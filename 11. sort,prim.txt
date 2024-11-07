import sys

def selection_sort(arr):
    for i in range(len(arr)):
        min_idx = min(range(i, len(arr)), key=arr.__getitem__)
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    print("Sorted array:", arr)

def prim_mst(graph, vertices):
    num_vertices = len(vertices)
    selected = [False] * num_vertices
    selected[0] = True
    mst_edges = []
    total_weight = 0

    for _ in range(num_vertices - 1):
        min_edge = sys.maxsize
        u = v = -1
        for i in range(num_vertices):
            if selected[i]:
                for j in range(num_vertices):
                    if not selected[j] and graph[i][j] is not None:
                        weight = graph[i][j][1]
                        if weight < min_edge:
                            min_edge, u, v = weight, i, j
        selected[v] = True
        mst_edges.append((vertices[u], vertices[v], min_edge))
        total_weight += min_edge

    print("MST Edges:")
    for edge in mst_edges:
        print(f"{edge[0]} -- {edge[1]} with weight {edge[2]}")
    print("Total weight of the MST:", total_weight)

def menu_selection_prim():
    while True:
        choice = input("\nMenu:\n1. Selection Sort\n2. Prim's MST\n3. Exit\nEnter your choice: ")
        if choice == '1':
            arr = list(map(int, input("Enter numbers separated by space: ").split()))
            selection_sort(arr)
        elif choice == '2':
            edges = []
            vertices_set = set()
            num_edges = int(input("Enter number of edges: "))
            for _ in range(num_edges):
                u, v, weight = input("Enter edge (u v weight): ").split()
                edges.append((u, v, int(weight)))
                vertices_set.update([u, v])

            vertices = list(vertices_set)
            num_vertices = len(vertices)
            graph = [[None for _ in range(num_vertices)] for _ in range(num_vertices)]
            for u, v, weight in edges:
                u_idx, v_idx = vertices.index(u), vertices.index(v)
                graph[u_idx][v_idx] = (v, weight)
                graph[v_idx][u_idx] = (u, weight)

            prim_mst(graph, vertices)
        elif choice == '3':
            print("Exiting...")
            break

if __name__ == "__main__":
    menu_selection_prim()
