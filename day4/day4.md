Day 4
================

## Task 1

``` clojure
(def data (r/readLines "input.txt"))
(defn parse-kv [kv-str] 
  (let [split (first (r/strsplit kv-str ":" :fixed true))]
    (assoc {} (first split) (get split 2))))
(defn parse-line [line] 
  (let [split (first (r/strsplit line " " :fixed true))]
    (reduce conj (map parse-kv split) {})))
(defn pp-valid? [passport]
  (or 
    (= (count passport) 8)
    (and (= (count passport) 7) (not (contains? passport "cid")))))
(def passports 
  (reduce 
    (fn [acc el]
      (if (= el "")
        (conj acc [{}])
        (let [passport (get acc (count acc))
              new-vals (parse-line el)]
          (assoc acc (count acc) (conj passport new-vals))))) data [{}]))
(->>
  passports
  (filter pp-valid?)
  (count))
```

    ## [1] 260

## Task 2

``` clojure
(defn between [x a b]
 (and (>= x (int a)) (<= x (int b))))
(defn rm-unit [x] (r/substr x 1 (- (r/nchar x) 1)))
(defn grepl [pattern x] 
  (if (r/is.null x) false (r/base::grepl pattern x)))
(defn four-digits? [x] (grepl "^[0-9]{4}$" x))
(defmacro check-field [field check]
  `(let [~field (get passport ~(str field))] 
    (and (not (r/is.null ~field)) ~check)))
(defmacro check-year [field lower upper]
  `(check-field ~field (and (four-digits? ~field) (between ~field ~lower ~upper))))
(defmacro check-in-set [field set]
  `(check-field ~field (r/%in% ~field (quote ~(map str set)))))
(defn extract-number [str] 
  (int (let [len (r/nchar str)] (r/substr str 1 (- len 2)))))
(defn extract-unit [str] 
  (let [len (r/nchar str)] (r/substr str (- len 1) len)))
(defn pp-strict-valid? [passport]
  (and 
    (pp-valid? passport)
    (check-year byr 1920 2002)
    (check-year iyr 2010 2020)
    (check-year eyr 2020 2030)
    (check-field hgt 
      (let [num (extract-number hgt) unit (extract-unit hgt)]
        (and (> (r/nchar hgt) 2) 
          (if (= unit "cm") (between num 150 193) (between num 59 76)))))
    (check-field hcl (grepl "^#[0-9a-f]{6}$" hcl))
    (check-in-set ecl (amb blu brn gry grn hzl oth))
    (check-field pid (grepl "^[0-9]{9}$" pid))))

(->>
  passports
  (filter pp-strict-valid?)
  (count))
```

    ## [1] 153
