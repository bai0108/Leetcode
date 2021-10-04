# BackTracking

> Combination problems, to find all posibilities.

55. N Queens 

[LeetCode](https://leetcode.com/problems/n-queens/)

```java
class Solution {
    private int size;
    private List<List<String>> solutions = new ArrayList<List<String>>();
    
    public List<List<String>> solveNQueens(int n) {
        size = n;
        char emptyBoard[][] = new char[size][size];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                emptyBoard[i][j] = '.';
            }
        }

        backtrack(0, new HashSet<>(), new HashSet<>(), new HashSet<>(), emptyBoard);
        return solutions;
    }
    
    // Making use of a helper function to get the
    // solutions in the correct output format
    private List<String> createBoard(char[][] state) {
        List<String> board = new ArrayList<String>();
        for (int row = 0; row < size; row++) {
            String current_row = new String(state[row]);
            board.add(current_row);
        }
        
        return board;
    }
    
    private void backtrack(int row, Set<Integer> diagonals, Set<Integer> antiDiagonals, 
                            Set<Integer> cols, char[][] state) {
        // Base case - N queens have been placed
        if (row == size) {
            solutions.add(createBoard(state));
            return;
        }
        
        for (int col = 0; col < size; col++) {
            int currDiagonal = row - col;
            int currAntiDiagonal = row + col;
            // If the queen is not placeable
            if (cols.contains(col) || diagonals.contains(currDiagonal) || antiDiagonals.contains(currAntiDiagonal)) {
                continue;    
            }
            
            // "Add" the queen to the board
            cols.add(col);
            diagonals.add(currDiagonal);
            antiDiagonals.add(currAntiDiagonal);
            state[row][col] = 'Q';

            // Move on to the next row with the updated board state
            backtrack(row + 1, diagonals, antiDiagonals, cols, state);

            // "Remove" the queen from the board since we have already
            // explored all valid paths using the above function call
            cols.remove(col);
            diagonals.remove(currDiagonal);
            antiDiagonals.remove(currAntiDiagonal);
            state[row][col] = '.';
        }
    }
}
```

52. N-Queens II

[LeetCode](https://leetcode.com/problems/n-queens-ii/)

``` java
class Solution {
    private int size, ans;
    private void backTracking(int row, Set<Integer> cols, Set<Integer> diagnals, Set<Integer> antiDiagnals) {
        if(row == size) {
            ans++;
            return;
        }
        
        for(int col = 0; col < size; col++) {
            int diagnal = row - col;
            int antiDiagnal = row + col;
            if(cols.contains(col) || diagnals.contains(diagnal) || antiDiagnals.contains(antiDiagnal))
                continue;
            
            cols.add(col);
            diagnals.add(diagnal);
            antiDiagnals.add(antiDiagnal);
            
            backTracking(row+1, cols, diagnals, antiDiagnals);
            
            cols.remove(col);
            diagnals.remove(diagnal);
            antiDiagnals.remove(antiDiagnal);
        }
    }
    
    public int totalNQueens(int n) {
        size = n;
        ans = 0;
        
        
        backTracking(0, new HashSet<Integer>(), new HashSet<Integer>(), new HashSet<Integer>());
        
        return ans;
        
    }
}
```

17. Letter Combinations of a Phone Number

[leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```java
class Solution {
    private List<String> combinations = new ArrayList<>();
    private Map<Character, String> letters = Map.of(
        '2', "abc", '3', "def", '4', "ghi", '5', "jkl", 
        '6', "mno", '7', "pqrs", '8', "tuv", '9', "wxyz");
    private String phoneDigits;
    
    public List<String> letterCombinations(String digits) {
        // If the input is empty, immediately return an empty answer array
        if (digits.length() == 0) {
            return combinations;
        }
        
        // Initiate backtracking with an empty path and starting index of 0
        phoneDigits = digits;
        backtrack(0, new StringBuilder());
        return combinations;
    }
    
    private void backtrack(int index, StringBuilder path) {
        // If the path is the same length as digits, we have a complete combination
        if (path.length() == phoneDigits.length()) {
            combinations.add(path.toString());
            return; // Backtrack
        }
        
        // Get the letters that the current digit maps to, and loop through them
        String possibleLetters = letters.get(phoneDigits.charAt(index));
        for (char letter: possibleLetters.toCharArray()) {
            // Add the letter to our current path
            path.append(letter);
            // Move on to the next digit
            backtrack(index + 1, path);
            // Backtrack by removing the letter before moving onto the next
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```

22. Generate Parentheses

[leetcode](https://leetcode.com/problems/generate-parentheses/)

```java
class Solution {

    private void backTracking(List<String> ans, StringBuilder cur, int open, int close, int max) {
        if(cur.length() == max*2){
            ans.add(cur.toString());
            return;
        }
        
        if(open < max) {
            cur.append("(");
            backTracking(ans, cur, open + 1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if(close < open){
            cur.append(")");
            backTracking(ans, cur, open, close + 1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
    
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        backTracking(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }
}
```
