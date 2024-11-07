from collections import defaultdict, deque

def dfs_preorder(graph, node, visited=set()):
    if node in visited:
        return
    visited.add(node)
    print(node, end=' ')
    for neighbor in graph[node]:
        dfs_preorder(graph, neighbor, visited)

def dfs_inorder(graph, node, visited=set()):
    if node in visited:
        return
    visited.add(node)
    neighbors = graph[node]
    if len(neighbors) > 1:
        dfs_inorder(graph, neighbors[0], visited)  # Visit left child
        print(node, end=' ')  # Visit node
        dfs_inorder(graph, neighbors[1], visited)  # Visit right child
    elif len(neighbors) == 1:
        dfs_inorder(graph, neighbors[0], visited)  # Visit single child
        print(node, end=' ')
    else:
        print(node, end=' ')

def dfs_postorder(graph, node, visited=set()):
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs_postorder(graph, neighbor, visited)
    print(node, end=' ')

def bfs_algorithm(graph, start_node):
    visited, queue = set(), deque([start_node])
    while queue:
        node = queue.popleft()
        if node not in visited:
            visited.add(node)
            print(node, end=' ')
            queue.extend(neighbor for neighbor in graph[node] if neighbor not in visited)

def create_graph():
    graph = defaultdict(list)
    for _ in range(int(input("Enter the number of nodes: "))):
        node = input("Enter the node: ").strip()
        for _ in range(int(input(f"Enter the number of neighbors for {node}: "))):
            graph[node].append(input(f"Enter the neighbor of {node}: ").strip())
    return graph

def display_nodes(index_to_node):
    print("\nAvailable nodes:")
    for i, node in index_to_node.items():
        print(f"{i}. {node}")

def main():
    graph = create_graph()
    index_to_node = {i: node for i, node in enumerate(graph.keys())}
    
    while True:
        display_nodes(index_to_node)  # Display nodes before the menu
        choice = int(input("\nMenu:\n1. Perform DFS\n2. Perform BFS\n3. Exit\nEnter your choice: "))
        
        if choice == 1:
            start_node = index_to_node[int(input("Select starting node index: "))]
            traversal_choice = int(input("Select traversal type (0: Pre, 1: In, 2: Post): "))
            if traversal_choice == 0:
                print("\nDFS Traversal (Preorder):", end=' ')
                dfs_preorder(graph, start_node)
            elif traversal_choice == 1:
                print("\nDFS Traversal (Inorder):", end=' ')
                dfs_inorder(graph, start_node)
            elif traversal_choice == 2:
                print("\nDFS Traversal (Postorder):", end=' ')
                dfs_postorder(graph, start_node)
            else:
                print("Invalid choice. Please select 0, 1, or 2.")
        
        elif choice == 2:
            start_node = index_to_node[int(input("Select starting node index: "))]
            print("\nBFS traversal:", end=' ')
            bfs_algorithm(graph, start_node)
        
        elif choice == 3:
            print("Exiting...")
            break

if __name__ == "__main__":
    main()
