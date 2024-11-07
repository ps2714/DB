class Graph:
    def __init__(self, vertices_count):
        self.V = vertices_count
        self.graph = []

    def add_edge(self, u, v, w):
        self.graph.append((w, u, v))

    def find(self, parent, i):
        if parent[i] == i:
            return i
        parent[i] = self.find(parent, parent[i])
        return parent[i]

    def union(self, parent, rank, x, y):
        root_x, root_y = self.find(parent, x), self.find(parent, y)
        if rank[root_x] < rank[root_y]:
            parent[root_x] = root_y
        elif rank[root_x] > rank[root_y]:
            parent[root_y] = root_x
        else:
            parent[root_y] = root_x
            rank[root_x] += 1

    def kruskal_mst(self):
        self.graph.sort()
        parent = list(range(self.V))
        rank = [0] * self.V
        mst = []

        for w, u, v in self.graph:
            if self.find(parent, u) != self.find(parent, v):
                mst.append((u, v, w))
                self.union(parent, rank, u, v)

        print("\nEdges in the MST:")
        for edge in mst:
            print(f"{edge[0]} -- {edge[1]} with weight {edge[2]}")
        print("Total weight of the MST:", sum(w for _, _, w in mst))

def selection_sort(arr):
    for i in range(len(arr)):
        min_idx = min(range(i, len(arr)), key=arr.__getitem__)
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    print("Sorted array:", arr)

def menu_selection_kruskal():
    while True:
        choice = input("\nMenu:\n1. Selection Sort\n2. Kruskal's MST\n3. Exit\nEnter your choice: ")
        if choice == '1':
            arr = list(map(int, input("Enter numbers separated by space: ").split()))
            selection_sort(arr)
        elif choice == '2':
            edges = []
            vertices_set = set()
            num_edges = int(input("Enter number of edges: "))
            for _ in range(num_edges):
                u, v, w = input("Enter edge (u v w): ").split()
                edges.append((u, v, int(w)))
                vertices_set.update([u, v])

            vertices = list(vertices_set)
            num_vertices = len(vertices)
            g = Graph(num_vertices)

            # Convert vertex labels to indices and add edges to the graph
            for u, v, w in edges:
                g.add_edge(vertices.index(u), vertices.index(v), w)

            g.kruskal_mst()
        elif choice == '3':
            print("Exiting...")
            break

if __name__ == "__main__":
    menu_selection_kruskal()
