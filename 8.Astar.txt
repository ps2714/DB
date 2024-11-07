import heapq

def a_star(graph, start, end, heuristic):
    open_list = [(0, start)]
    g_cost, f_cost, came_from = {start: 0}, {start: heuristic[start]}, {start: None}
    
    while open_list:
        current_priority, current_node = heapq.heappop(open_list)
        if current_node == end:
            path, cost = [], g_cost[end]
            while current_node: path.append(current_node); current_node = came_from[current_node]
            return path[::-1], cost
        
        for neighbor, weight in graph[current_node].items():
            tentative_g_cost = g_cost[current_node] + weight
            if neighbor not in g_cost or tentative_g_cost < g_cost[neighbor]:
                g_cost[neighbor], f_cost[neighbor], came_from[neighbor] = tentative_g_cost, tentative_g_cost + heuristic[neighbor], current_node
                heapq.heappush(open_list, (f_cost[neighbor], neighbor))
    
    return None, float('inf')

def main():
    graph, heuristic = {}, {}
    for _ in range(int(input("Enter the number of Nodes: "))):
        node = input("Enter the Node: ").strip()
        graph[node] = {}
        heuristic[node] = int(input(f"Heuristic value for {node}: "))
    for _ in range(int(input("Enter the number of Edges: "))):
        u, v, w = input("Enter the Edges in format (u v weight): ").strip().split()
        graph[u][v] = graph[v][u] = int(w)
    start, end = input(" Enter the Start Node : ").strip(), input(" Enter the End Node: ").strip()
    
    if start not in graph or end not in graph:
        print("Invalid start or end node.")
        return
    
    path, cost = a_star(graph, start, end, heuristic)
    print(f"Shortest path: {' -> '.join(path) if path else 'No path found.'}")
    print(f"Total cost: {cost if path else 'N/A'}")

if __name__ == "__main__":
    main()
