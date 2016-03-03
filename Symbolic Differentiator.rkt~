#lang racket

;;----------List Manipulations---------------------
;;Find a symbol in a list
(define (memq item x)
  (cond ((null? x) false)
        ((eq? item (car x)) x)
        (else (memq item (cdr x)))))

;;Append two lists
(define (append list1 list2)
  (if(null? list1) list2
     (cons (car list1) (append (cdr list1) list2))))

;;Length of a lists
(define (length item)
  (if(null? item) 0
     (+ 1 (length (cdr item)))))

;;--------To Convert an expression---------------
(define (is-bi-oper? item)
  (or (eq? '+ item)
      (eq? '* item)
      (eq? '^ item)
      (eq? '- item)
      (eq? '/ item)))

(define (pre-to-in item)
  (cond ((null? item) '())
        ((not (pair? item)) item)
        (else
         (if(is-bi-oper? (car item))
            (list (pre-to-in (cadr item)) (car item) (pre-to-in (caddr item)))
            (list (car item) (pre-to-in (cadr item)))))))

(define (in-to-pre item)
  
  (cond ((null? item) '())
        ((not (pair? item)) item)
        ((eq? (car item) 'log)
         (make-log (in-to-pre (log-base item)) (in-to-pre (log-num item))))
        (else
         (if(is-bi-oper? (cadr item))
            (list (cadr item) (in-to-pre (car item)) (in-to-pre (caddr item)))
            (list (car item) (in-to-pre (cadr item)))))))

;;------------For symbolic differentiation---------------

;Variable identifiers
(define (variable? exp) (symbol? exp))

(define (same-variable? exp1 exp2)
  (and (variable? exp1) (variable? exp2) (eq? exp1 exp2)))

(define (=number? exp num)
  (and (number? exp) (= exp num)))

;;Templates for binary operators
(define (is-oper-temp? oper exp)
  (and (pair? exp) (eq? (car exp) oper)))

(define (first-operand oper exp)
  (if(is-oper-temp? oper exp) (cadr exp)
     (error "The given exp is not a binary operator" exp)))

(define (second-operand oper exp)
  (if(is-oper-temp? oper exp)
     (if(null? (cdddr exp)) (caddr exp) (append (list oper) (cddr exp)))
     (error "The given exp is not a binary operator" exp)))

;;Sums manipulations
(define (sum? exp) (is-oper-temp? '+ exp))
(define (addend exp) (first-operand '+ exp))
(define (augend exp) (second-operand '+ exp))

(define (make-sum exp1 exp2)
  (cond ((=number? exp1 0) exp2)
        ((=number? exp2 0) exp1)
        ((and (number? exp1) (number? exp2)) (+ exp1 exp2))
        (else (list '+ exp1 exp2))))

;;Product manipulations
(define (product? exp) (is-oper-temp? '* exp))
(define (multiplier exp) (first-operand '* exp))
(define (multiplicand exp) (second-operand '* exp))

(define (make-product exp1 exp2)
  (cond ((=number? exp1 1) exp2)
        ((=number? exp2 1) exp1)
        ((or (=number? exp1 0) (=number? exp2 0)) 0)
        ((and (number? exp1) (number? exp2)) (* exp1 exp2))
        (else (list '* exp1 exp2))))

;;Exponent manipulations
(define (expo? exp) (is-oper-temp? '^ exp))
(define (expo-base exp) (first-operand '^ exp))
(define (expo-pow exp) (second-operand '^ exp))

(define (make-expo base pow)
  (cond ((=number? base 1) 1)
        ((=number? pow 1) base)
        ((=number? base 0) 0)
        ((=number? pow 0) 1)
        (else (list '^ base pow))))

;;Logarithmic Manipulations
(define (is-log? exp)  (is-oper-temp? 'log exp))
(define (log-base exp) (first-operand 'log exp))
(define (log-num exp) (second-operand 'log exp))

(define (valid-log base num)
  (cond ((and (number? base) (< base 0)) ((error "Base cannot be negative") false))
        ((and (number? base) (= base 1))  ((error "Base cannot be equal to one") false))
        ((=number? num 0) ((error "log goes to negative infintiy argument = 0") false))
        ((and (number? num) (< num 0)) ((error "Argument cannot be negative") false))
        (else true)))

(define (make-log base num)
  (if(valid-log base num)
     (cond ((eq? base num) 1)
           ((=number? num 1) 0)
           (else (list 'log base num)))
     (error "given combination results in an invalid log")))

;Natural Log Manipulations
(define (is-nat-log? exp) (is-oper-temp? 'ln exp))

(define (make-nat-log exp)
  (if (valid-log 'e exp)
      (cond ((eq? 'e exp) 1)
            ((=number? exp 1) 0)
            (else (list 'ln exp)))
      (error "The given log is invlaid")))


(define (nat-log-num exp) (first-operand 'ln exp))

;To convert normal logs to Natural (base 'e') logs
(define (nat-log exp)
  (if(not (is-log? exp)) (error "The given expression is not a log")
     (make-product (make-nat-log (log-num exp)) (make-expo (make-nat-log (log-base exp)) -1))))


;;Triginometric Derivatives
(define (trig-deriv exp)
  (let ((func (car exp)))
    (cond ((eq? func 'sin)
           (append (list 'cos) (cdr exp)))
          
          ((eq? func 'cos)
           (make-product (append (list 'sin) (cdr exp)) -1))
          
          ((eq? func 'tan)
           (make-expo (append (list 'sec) (cdr exp)) 2))
          
          ((eq? func 'cot)
           (make-product (make-expo (append (list 'csc) (cdr exp)) 2) -1))
          
          ((eq? func 'sec)
           (make-product (append (list 'sec) (cdr exp)) (append (list 'tan) (cdr exp))))
          
          ((eq? func 'csc)
           (make-product (make-product (append (list 'csc) (cdr exp)) (append (list 'cot) (cdr exp))) -1)))))

(define (is-trig? exp)
  (or (is-oper-temp? 'cos exp)
      (is-oper-temp? 'sin exp)
      (is-oper-temp? 'tan exp)
      (is-oper-temp? 'cot exp)
      (is-oper-temp? 'sec exp)
      (is-oper-temp? 'csc exp)))
;;Function to keep track of known derivalbe functions
(define (is-known-func? exp)
  (or (eq? exp 'cos)
      (eq? exp 'sin)
      (eq? exp 'tan)
      (eq? exp 'cot)
      (eq? exp 'sec)
      (eq? exp 'csc)
      (eq? exp 'log)
      (eq? exp 'ln)))

;;----Function to take partial-derivative w.r.t var-----

(define (par-deriv infix-exp var)
  (define (prefix-par-deriv exp var)
    
    (cond ((number? exp) 0) ;;Base Cases
          ((variable? exp) (if (same-variable? exp var) 1 0))
          
          ;;Implementations of Separation Rules
          ((is-bi-oper? (car exp))          
           (cond ((sum? exp) (make-sum (prefix-par-deriv (addend exp) var)
                                       (prefix-par-deriv (augend exp) var)))
                 ((product? exp) (make-sum
                                  (make-product (multiplier exp) (prefix-par-deriv (multiplicand exp) var))
                                  (make-product (multiplicand exp) (prefix-par-deriv (multiplier exp) var))))
                 ((expo? exp) (make-product (expo-pow exp) (prefix-par-deriv (make-product (expo-pow exp) (make-nat-log (expo-base exp))) var)))))

          ;;Implementation of Chain Rule
          ((is-known-func? (car exp))
           (make-product (prefix-par-deriv (first-operand (car exp) exp) var)
                         (cond ((is-trig? exp) (trig-deriv exp))
                               ((is-nat-log? exp) (make-expo (nat-log-num exp) -1))
                               ((is-log? exp) (prefix-par-deriv (nat-log exp) var))
                               (else (error "unknown expression type: DERIV" exp)))))      
          
          (else (error "unknown expression type: DERIV" exp))))
  
  (pre-to-in (prefix-par-deriv (in-to-pre infix-exp) var)))


;;Returns the nth partial of a expression for given variable list
(define (n-partial exp var-list)
  (if(= (length var-list) 1) (par-deriv exp (car var-list))
     (par-deriv (n-partial exp (cdr var-list)) (car var-list))))

(n-partial '(x ^ x) '(x))

;;(make-product (expo-pow exp) (prefix-par-deriv (make-product (expo-pow exp) (make-nat-log (expo-base exp)))))