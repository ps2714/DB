import heapq

class Graph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, u, v, w):
        if u not in self.graph:
            self.graph[u] = []
        if v not in self.graph:
            self.graph[v] = []
        self.graph[u].append((v, w))
        self.graph[v].append((u, w))  # Remove this line if the graph is directed

    def dijkstra(self, start, end):
        distances = {vertex: float('infinity') for vertex in self.graph}
        distances[start] = 0
        priority_queue = [(0, start)]
        predecessors = {vertex: None for vertex in self.graph}

        while priority_queue:
            current_distance, u = heapq.heappop(priority_queue)

            if current_distance > distances[u]:
                continue

            for neighbor, weight in self.graph[u]:
                distance = current_distance + weight

                if distance < distances[neighbor]:
                    distances[neighbor] = distance
                    predecessors[neighbor] = u
                    heapq.heappush(priority_queue, (distance, neighbor))

        # Reconstruct the shortest path
        path = []
        current = end
        while current is not None:
            path.append(current)
            current = predecessors[current]
        path.reverse()

        if distances[end] == float('infinity'):
            return "No path", []

        return distances[end], path

def main():
    g = Graph()
    
    while True:
        print("\nMenu:")
        print("1. Add Edge")
        print("2. Find Shortest Path")
        print("3. Exit")
        choice = input("Enter choice: ")

        if choice == '1':
            u = input("Enter vertex u: ")
            v = input("Enter vertex v: ")
            w = float(input("Enter weight of edge u-v: "))
            g.add_edge(u, v, w)
            print(f"Edge added: {u} - {v} with weight {w}")

        elif choice == '2':
            start = input("Enter starting vertex: ")
            end = input("Enter ending vertex: ")
            distance, path = g.dijkstra(start, end)
            if distance == "No path":
                print(f"Shortest path from {start} to {end} is: No path")
            else:
                print(f"Shortest path from {start} to {end} is: {distance}")
                print("Path:", " -> ".join(path))

        elif choice == '3':
            print("Exiting...")
            break

        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
