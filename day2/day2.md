Day 2
================

## Task 1

``` clojure
(defn parse-policy-range [range]
  (let [str_split (first (r/strsplit :x range :split "-" :fixed true))]
    [(r/as.integer (get str_split 1)) (r/as.integer (get str_split 2))]
    ))
(defn parse-character [char_str] (r/substr char_str 1 1))
(defn parse-policy-line [line]
  (let [str_split (first (r/strsplit :x line :split " " :fixed true))]
    {:range (parse-policy-range (get str_split 1))
     :character (parse-character (get str_split 2))
     :password (get str_split 3)}
    ))
(defn count-char [char str] 
  (- (r/nchar str) (r/nchar (r/gsub char "" str :fixed true))))
(defn valid? [policy] 
  (let [char-count (count-char (get policy :character) (get policy :password))
        range (get policy :range)]
    (and (>= char-count (get range 1)) (<= char-count (get range 2)))))

(->>
  (r/readLines "input.txt")
  (map parse-policy-line)
  (filter valid?)
  (count))
```

    ## [1] 418

## Task 2

``` clojure
(defn char-at [str i] (r/substr str i i))
(defn xor [a b] (and (or a b) (not (and a b))))
(defn valid2? [policy] 
  (let [range (get policy :range)
        password (get policy :password)
        char (get policy :character)
        idx1 (get range 1)
        idx2 (get range 2)
        char1 (char-at password idx1)
        char2 (char-at password idx2)]
    (xor (= char char1) (= char char2))))

(->>
  (r/readLines "input.txt")
  (map parse-policy-line)
  (filter valid2?)
  (count))
```

    ## [1] 616
