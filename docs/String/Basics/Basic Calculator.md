# 224. Basic Calculator
https://leetcode.com/problems/basic-calculator/


~~~java

class Solution {
    public int calculate(String s) {
        if (s == null || s.isEmpty()) {
            return 0;
        }
        
        int sign = 1;
        int ans = 0;
        int i = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        
        while (i < s.length()) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int num = 0;
                while (i < s.length() && Character.isDigit(s.charAt(i))) {
                    num = num * 10 + s.charAt(i) - '0';
                    i++;
                }
                ans = ans + sign * num;
                continue;
            } else if (c == '+') {
                sign = 1;
            } else if (c == '-') {
                sign = -1;
            } else if (c == '(') {
                stack.offerFirst(ans);
                stack.offerFirst(sign);
                ans = 0;
                sign = 1;
            } else if (c == ')') {
                ans = stack.pollFirst() * ans + stack.pollFirst();
            } 
            i++;
        }
        
        return ans;
    }
}
    
~~~


# 227. Basic Calculator II
https://leetcode.com/problems/basic-calculator-ii/


~~~java

class Solution {
    public int calculate(String s) {
        if (s == null || s.isEmpty()) {
            return 0;
        }
        
        int i = 0;
        char sign = '+';
        Deque<Integer> stack = new ArrayDeque<>();
        while (i < s.length()) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int num = 0;
                while (i < s.length() && Character.isDigit(s.charAt(i))) {
                    num = num * 10 + s.charAt(i) - '0';
                    i++;
                }
                if (sign == '+') stack.offerFirst(num);
                else if (sign == '-') stack.offerFirst(-num);
                else if (sign == '*') stack.offerFirst(stack.pollFirst() * num);
                else stack.offerFirst(stack.pollFirst() / num);
                continue;
            } else if (c == '+' || c == '-' || c == '*' || c == '/') {
                sign = c;
            }
            i++;
        }
        
        int ans = 0;
        while (!stack.isEmpty()) {
            ans += stack.pollFirst();
        }
        return ans;
    }
}

~~~

# 772. Basic Calculator III

https://leetcode.com/problems/basic-calculator-iii/


~~~java

class Solution {
    public int calculate(String s) {
        if (s == null || s.isEmpty()) {
            return 0;
        }
        
        int i = 0;
        char sign = '+';
        Deque stack = new ArrayDeque<>();
        while (i < s.length()) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int num = 0;
                while (i < s.length() && Character.isDigit(s.charAt(i))) {
                    num = num * 10 + s.charAt(i) - '0';
                    i++;
                }
                helper(stack, num, sign);
                continue;
            } else if (c == '+' || c == '-' || c == '*' || c == '/') {
                sign = c;
            } else if (c == '(') {
                stack.offerFirst(sign);
                stack.offerFirst('(');
                sign = '+';
            } else if (c == ')') {
                int num = 0;
                while (!stack.isEmpty() && (stack.peekFirst() instanceof Integer)) {
                    num = num + (Integer) stack.pollFirst();
                }
                // poll out '('
                stack.pollFirst();
                // sign is on the top now
                sign = (char) stack.pollFirst();
                helper(stack, num, sign);
            }
            i++;
        }
        
        int ans = 0;
        while (!stack.isEmpty()) {
            ans = ans + (Integer) stack.pollFirst();
        }
        return ans;
    }
    
    private void helper(Deque stack, int num, char sign) {
        if (sign == '+') stack.offerFirst(num);
        else if (sign == '-') stack.offerFirst(-num);
        else if (sign == '*') stack.offerFirst((int) stack.pollFirst() * num);
        else stack.offerFirst((int) stack.pollFirst() / num);
    }
}
~~~
