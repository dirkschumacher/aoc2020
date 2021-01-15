Day 3
================

## Task 1

``` clojure
(def data (r/readLines "input.txt"))
(def char-at (fn [str i] (r/substr str i i)))
(def line-length (count data))
(def line-at (fn [i] (get data i)))
(def line-width (r/nchar (get data 1)))
(defn find-trees [down right]
  (loop [i (+ 1 down) j (+ 1 right) n-trees (r/as.numeric 0)] ; 32 bit integers :)
    (if (> i line-length) n-trees 
      (let 
        [str (line-at i) 
         char (char-at str j)
         new-i (+ i down) 
         new-j (+ 1 (mod (+ j (- right 1)) line-width))]
        (recur new-i new-j (if (= char "#") (+ n-trees 1) n-trees))))))
(find-trees 1 3)
```

    ## 214

## Task 2

``` clojure
(*
  (find-trees 1 1)
  (find-trees 1 3)
  (find-trees 1 5)
  (find-trees 1 7)
  (find-trees 2 1))
```

    ## 8336352024
