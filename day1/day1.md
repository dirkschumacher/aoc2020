Day 1
================

## Task 1

``` clojure
(def numbers (r/as.integer (r/readLines "input.txt")))
(def map2 (fn [f x y] (r/purrr::map2 x y f)))
(def combs (r/expand.grid :a numbers :b numbers))
(def res 
  (first (->> 
    (map2 (fn [a b] [a b (+ a b)]) (r/$ combs a) (r/$ combs b))
    (filter (fn [x] (= (get x 3) 2020))))))
res
```

    ## [196 1824 2020]

``` clojure
(* (get res 1) (get res 2))
```

    ## [1] 357504

## Task 2

With dplyr

``` clojure
(-> 
 (r/expand.grid :a numbers :b numbers :c numbers)
 (r/dplyr::filter (r/== (r/+ (r/+ a b) c) 2020))
 (r/dplyr::mutate :result (r/* (r/* a b) c))
 (r/head 1))
```

    ##     a    b  c   result
    ## 1 694 1312 14 12747392
