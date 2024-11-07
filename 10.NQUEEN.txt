class BnB:
    def solveNQueens(self, N):
        if N < 1 or N > 10 or N in [2, 3]: 
            return print(f"No solutions for N={N}")
        solutions = set()
        
        def solve(col, board, rowLookup, slashLookup, backslashLookup):
            if col >= N:
                solutions.add(tuple(board))
                return
            for i in range(N):
                if not (rowLookup[i] or slashLookup[i + col] or backslashLookup[i - col + (N - 1)]):
                    board.append(i)
                    rowLookup[i] = slashLookup[i + col] = backslashLookup[i - col + (N - 1)] = True
                    solve(col + 1, board, rowLookup, slashLookup, backslashLookup)
                    rowLookup[i] = slashLookup[i + col] = backslashLookup[i - col + (N - 1)] = False
                    board.pop()
        
        solve(0, [], [False] * N, [False] * (2 * N - 1), [False] * (2 * N - 1))
        for idx, sol in enumerate(solutions, 1):
            print(f"Solution {idx}:")
            for i in sol:
                print(" ".join("Q" if j == i else "." for j in range(N)))
            print()
        print(f"Total solutions: {len(solutions)}\n")

class BT:
    def solveNQueens(self, N):
        if N < 1 or N > 10 or N in [2, 3]: 
            return print(f"No solutions for N={N}")
        solutions = set()
        
        def solve(col, board):
            if col >= N:
                solutions.add(tuple(board))
                return
            for i in range(N):
                if all(board[j] != i and abs(board[j] - i) != col - j for j in range(col)):
                    board.append(i)
                    solve(col + 1, board)
                    board.pop()
        
        solve(0, [])
        for idx, sol in enumerate(solutions, 1):
            print(f"Solution {idx}:")
            for i in sol:
                print(" ".join("Q" if j == i else "." for j in range(N)))
            print()
        print(f"Total solutions: {len(solutions)}\n")

def main():
    while True:
        print("N-Queens Problems\n1. Branch and Bound\n2. Backtracking\n3. Exit\nEnter your choice:")
        choice = int(input())
        if choice == 3: break
        N = int(input("Enter board size (1-10): "))
        (BnB() if choice == 1 else BT()).solveNQueens(N)

if __name__ == "__main__":
    main()
