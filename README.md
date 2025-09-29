import java.util.*;

class Edge implements Comparable<Edge> {
    int src, dest, weight;

    public Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }

    @Override
    public int compareTo(Edge other) {
        return this.weight - other.weight;
    }
}

class Graph {
    private int V;
    private List<Edge> edges;

    public Graph(int V) {
        this.V = V;
        edges = new ArrayList<>();
    }

    public void addEdge(int src, int dest, int weight) {
        edges.add(new Edge(src, dest, weight));
    }

    public int kruskalMST() {
        Collections.sort(edges);
        int[] parent = new int[V];
        Arrays.fill(parent, -1);
        int mstWeight = 0;

        for (Edge edge : edges) {
            int x = find(parent, edge.src);
            int y = find(parent, edge.dest);

            if (x != y) {
                mstWeight += edge.weight;
                union(parent, x, y);
            }
        }

        return mstWeight;
    }

    private int find(int[] parent, int i) {
        if (parent[i] == -1) return i;
        return find(parent, parent[i]);
    }

    private void union(int[] parent, int x, int y) {
        int xset = find(parent, x);
        int yset = find(parent, y);
        parent[xset] = yset;
    }

    public int primMST() {
        boolean[] inMST = new boolean[V];
        int[] key = new int[V];
        Arrays.fill(key, Integer.MAX_VALUE);
        key[0] = 0;
        int mstWeight = 0;

        for (int count = 0; count < V; count++) {
            int u = minKey(key, inMST);
            inMST[u] = true;
            mstWeight += key[u];

            for (Edge edge : edges) {
                if (edge.src == u && !inMST[edge.dest] && edge.weight < key[edge.dest]) {
                    key[edge.dest] = edge.weight;
                }
                if (edge.dest == u && !inMST[edge.src] && edge.weight < key[edge.src]) {
                    key[edge.src] = edge.weight;
                }
            }
        }

        return mstWeight;
    }

    private int minKey(int[] key, boolean[] inMST) {
        int min = Integer.MAX_VALUE, minIndex = -1;
        for (int v = 0; v < V; v++) {
            if (!inMST[v] && key[v] < min) {
                min = key[v];
                minIndex = v;
            }
        }
        return minIndex;
    }
}

public class Main {
    public static void main(String[] args) {
        int V = 6;
        Graph graph = new Graph(V);

        graph.addEdge(0, 1, 6);
        graph.addEdge(0, 2, 1);
        graph.addEdge(0, 3, 5);
        graph.addEdge(1, 2, 2);
        graph.addEdge(1, 4, 5);
        graph.addEdge(2, 3, 2);
        graph.addEdge(2, 4, 6);
        graph.addEdge(3, 4, 1);
        graph.addEdge(4, 5, 4);

        System.out.println("Peso total MST con Kruskal: " + graph.kruskalMST());
        System.out.println("Peso total MST con Prim: " + graph.primMST());
    }
}
