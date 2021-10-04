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
39. Combination Sum

[leetcode](https://leetcode.com/problems/combination-sum/)

```java
class Solution {
    List<List<Integer>> ans;
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        ans = new ArrayList<>();
        backTracking(0, 0, target, new ArrayList<Integer>(), candidates);
        return ans;
    }
    
    private void backTracking(int pos, int sum, int target, List<Integer> cur, int[] candid) {
        if(sum == target) {
            ans.add(new ArrayList<Integer>(cur));
            return;
        }
        
        for(int i = pos; i < candid.length;  i++) {
            sum += candid[i]; 
            if(sum > target) {
                sum -= candid[i];
                continue;
            }
            cur.add(candid[i]);
            backTracking(i, sum, target, cur, candid); 
            sum -= candid[i];
            cur.remove(cur.size()-1);
        }
    }
}
```

46. Permutations

[leetcode](https://leetcode.com/problems/permutations/)

```java
class Solution {
  public void backtrack(int n,
                        ArrayList<Integer> nums,
                        List<List<Integer>> output,
                        int first) {
    // if all integers are used up
    if (first == n)
      output.add(new ArrayList<Integer>(nums));
    for (int i = first; i < n; i++) {
      // place i-th integer first 
      // in the current permutation
      Collections.swap(nums, first, i);
      // use next integers to complete the permutations
      backtrack(n, nums, output, first + 1);
      // backtrack
      Collections.swap(nums, first, i);
    }
  }

  public List<List<Integer>> permute(int[] nums) {
    // init output list
    List<List<Integer>> output = new LinkedList();

    // convert nums into list since the output is a list of lists
    ArrayList<Integer> nums_lst = new ArrayList<Integer>();
    for (int num : nums)
      nums_lst.add(num);

    int n = nums.length;
    backtrack(n, nums_lst, output, 0);
    return output;
  }
}
```

78. Subsets

[leetcode](https://leetcode.com/problems/subsets/)

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        
        List<List<Integer>> ans = new ArrayList<>();
        ans.add(new ArrayList<Integer>());
        for(int num: nums) {
            List<List<Integer>> subset = new ArrayList<>();
            
            for(List<Integer> x: ans) {
                subset.add(new ArrayList<>(x));
                
            }
            
            for(List<Integer> x: subset) {
                //
                //ans.add(x.add(num)); error
                x.add(num);
                ans.add(x);
            }
            
            
        }
        return ans;
    }
}
```

79. Word Search

[leetcode](https://leetcode.com/problems/word-search/)

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        
        int rows = board.length;
        int cols = board[0].length;
        
        for(int y=0; y<rows; y++){
            for(int x=0; x<cols; x++){
                if(exist(board, word, y, x, 0)) return true;
            }
        }
        return false;
    }
    
    public boolean exist(char[][] board, String word, int y, int x, int i) {
        
        if(x < 0 || y < 0 || x > board[0].length-1 || y > board.length-1) return false;
        if(board[y][x] != word.charAt(i)) return false;
        
        if(i == word.length() - 1) return true;
        
        board[y][x] = '@';
        boolean result = exist(board, word, y-1, x, i+1)
            || exist(board, word, y+1, x, i+1)
            || exist(board, word, y, x-1, i+1)
            || exist(board, word, y, x+1, i+1);
        
        board[y][x] = word.charAt(i);   //Backtrack
        return result;
        
    }
}
```
