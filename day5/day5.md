Day 5
================

## Part 1

``` clojure
(ns aoc.day5.part1)
(def data (r/readLines "input.txt"))
(defn substring [str start end] (str (r/substr str start end)))
(defn str-to-vec [str] (first (r/strsplit str "")))
(defn binary-find [seq right-char bounds]
  (int (r/min 
    (reduce (fn [acc el] 
      (let [sum (- (get acc 2) (get acc 1)) sum2 (/ sum 2)]
        (if (= el right-char) 
          (assoc acc 1 (+ (get acc 1) (r/ceiling sum2)))
          (assoc acc 2 (- (get acc 2) (r/floor sum2)))))) seq bounds))))
(defn seat-id [seq]
  (let [row (binary-find (str-to-vec (substring seq 1 7)) "B" [0 127])
        col (binary-find (str-to-vec (substring seq 8 10)) "R" [0 7])]
    (+ (* row 8) col)))
  
(def seat-ids (map seat-id data))
(int (reduce r/max seat-ids))
```

    ## 858

## Part 2

``` clojure
(ns aoc.day5.part2)
(def seat-ids (filter (fn [x] (and (> x 7) (< x (* 127 8)))) aoc.day5.part1/seat-ids))
(def sorted-seats (r/sort (r/as.integer seat-ids)))
(loop [i 2] ; we assume the missing seat exists
  (let 
    [seat1 (get sorted-seats i) seat2 (get sorted-seats (- i 1))
      diff (- seat1 seat2)]
    (if (= diff 2)
      (+ seat2 1)
      (recur (inc i)))))
```

    ## 557
