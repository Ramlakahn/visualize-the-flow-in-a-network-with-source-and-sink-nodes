# visualize-the-flow-in-a-network-with-source-and-sink-nodes
import networkx as nx
import matplotlib.pyplot as plt

def visualize_network_flow():
    G = nx.DiGraph()

    num_edges = int(input("Enter the number of edges: "))
    print("Enter each edge in the format: source target capacity")
    edges = []

    for _ in range(num_edges):
        edge_input = input("Edge: ").split()
        if len(edge_input) != 3:
            print("Invalid input. Please enter exactly 3 values.")
            continue
        u, v, cap = edge_input
        try:
            cap = int(cap)
        except ValueError:
            print("Capacity must be an integer.")
            continue
        edges.append((u, v, cap))

    for u, v, capacity in edges:
        G.add_edge(u, v, capacity=capacity)

    source = input("Enter the source node: ")
    sink = input("Enter the sink node: ")

    flow_value, flow_dict = nx.maximum_flow(G, source, sink)

    edge_labels = {}
    edge_widths = []
    for u, v in G.edges():
        flow = flow_dict[u][v]
        capacity = G[u][v]['capacity']
        edge_labels[(u, v)] = f'{flow}/{capacity}'
        edge_widths.append(2 + 5 * (flow / capacity))

    pos = nx.spring_layout(G, seed=42)
    plt.figure(figsize=(10, 6))
    nx.draw(G, pos, with_labels=True, node_size=2000, node_color='lightblue',
            edge_color='gray', width=edge_widths)
    nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)

    nx.draw_networkx_nodes(G, pos, nodelist=[source], node_color='green', node_size=2500)
    nx.draw_networkx_nodes(G, pos, nodelist=[sink], node_color='red', node_size=2500)

    plt.title(f'Max Flow: {flow_value}')
    plt.show()
    return flow_value


if __name__ == "__main__":
    visualize_network_flow()
