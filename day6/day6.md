Day 6
================

## Part 1

  - No set support in llr yet

<!-- end list -->

``` clojure
(ns aoc.day6.part1)
(def data (r/readLines "input.txt"))
; we don't have sets yet, but we have maps
(->>
  data
  (reduce 
    (fn [acc el] 
      (let [coll (get acc (count acc))]
        (if (= el "")
          (conj acc [{}])
          (let 
            [chars (first (r/strsplit el ""))
             chars (map (fn [x] (assoc {} x true)) chars)
             key-map (reduce conj chars)]
             (assoc acc (count acc) (conj coll key-map)))))) [{}])
    (map count)
    (reduce +))
```

    ## [1] 6748

## Part 2

``` clojure
(ns aoc.day6.part2)

(->>
  aoc.day6.part1/data
  (reduce 
    (fn [acc el] 
      (let [record (get acc (count acc)) coll (get record :coll)]
        (if (= el "")
          (conj acc [{:coll {} :count 0}])
          (let 
            [chars (first (r/strsplit el ""))
             record (assoc record :count (inc (get record :count)))
             new-map 
              (reduce 
                (fn [a char] 
                  (assoc a char (if (contains? a char) (inc (get a char)) 1)))
                    coll chars)]
             (assoc acc (count acc) 
               (assoc record :coll new-map)))))) [{:coll {} :count 0}])
  (map 
    (fn [x]
      (let [count (get x :count)]
        (count (filter (fn [y] (= y count)) (vals (get x :coll)))))))
  (reduce +))
```

    ## [1] 3445
