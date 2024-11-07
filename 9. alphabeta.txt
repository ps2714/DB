def alpha_beta_pruning(depth, node_index, is_maximizing, values, alpha, beta, max_depth):
    # Base case: leaf node is reached
    if depth == max_depth:
        return values[node_index]

    if is_maximizing:
        best_value = float('-inf')

        # Recursively call for child nodes
        for i in range(2):  # Each node has 2 children
            value = alpha_beta_pruning(depth + 1, node_index * 2 + i, False, values, alpha, beta, max_depth)
            best_value = max(best_value, value)
            alpha = max(alpha, best_value)

            # Alpha-Beta Pruning
            if beta <= alpha:
                break

        return best_value

    else:
        best_value = float('inf')

        # Recursively call for child nodes
        for i in range(2):  # Each node has 2 children
            value = alpha_beta_pruning(depth + 1, node_index * 2 + i, True, values, alpha, beta, max_depth)
            best_value = min(best_value, value)
            beta = min(beta, best_value)

            # Alpha-Beta Pruning
            if beta <= alpha:
                break

        return best_value


def main():
    """
    Main function to run the Alpha-Beta Pruning example with user input.
    """
    print("Alpha-Beta Pruning Algorithm Example")
    print("===================================")
    print("1. Run Alpha-Beta Pruning")
    print("2. Exit")

    choice = input("Enter your choice (1 or 2): ")

    if choice == '1':
        # Get the number of leaf nodes
        leaf_node_count = int(input("Enter the number of leaf nodes (must be a power of 2): "))
        
        # Check if leaf_node_count is a power of 2
        if leaf_node_count & (leaf_node_count - 1) != 0:
            print("Invalid input. The number of leaf nodes must be a power of 2.")
            return

        # Calculate the maximum depth based on the number of leaf nodes
        max_depth = leaf_node_count.bit_length() - 1

        # Get leaf node values
        print(f"Enter {leaf_node_count} leaf node values (separated by space): ")
        values = list(map(int, input().split()))

        if len(values) != leaf_node_count:
            print(f"Invalid input. Please enter exactly {leaf_node_count} leaf node values.")
            return

        # Initialize Alpha and Beta
        alpha = float('-inf')
        beta = float('inf')

        print("Optimal value using Alpha-Beta Pruning:", alpha_beta_pruning(0, 0, True, values, alpha, beta, max_depth))

    elif choice == '2':
        print("Exiting...")
    else:
        print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
