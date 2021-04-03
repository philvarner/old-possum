# Regular Expressions


^ beginning of a line or string
$ end of a line or string. any character except newline
* 0 or more previous regular expression
*? 0 or more previous regular expression (non greedy)
+ 1 or more previous regular expression
+? 1 or more previous regular expression (non greedy)[] range specification (e.g.[a-z] a char in the range ‘a’ to ‘z’) \w an alphanumeric character\W a non-alphanumeric character\s a whitespace character\S a non-whitespace character\d a digit\D a non-digit character\b a backspace (when in a range specification)\b word boundary (when not in a range specification)\B non-word boundary* zero or more repetitions of the preceding+ one or more repetitions of the preceding{m,n} at least m and at most n repetitions of the preceding? at most one repetition of the preceding| either the preceding or next expression may match() a group 

{ x }, { x , y }, { x ,} -- counts of times