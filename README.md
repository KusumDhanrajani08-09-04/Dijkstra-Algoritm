# Dijkstra-Algoritm

import java.util.*;

class Graph {
    private int vertices; // Number of vertices in the graph
    private List<List<Node>> adjacencyList; // Adjacency list representation of the graph

    // Node class represents an edge in the graph
    class Node implements Comparable<Node> {
        int vertex;
        int weight;

        Node(int vertex, int weight) {
            this.vertex = vertex;
            this.weight = weight;
        }

        @Override
        public int compareTo(Node other) {
            return Integer.compare(this.weight, other.weight);
        }
    }

    // Constructor to initialize the graph
    Graph(int vertices) {
        this.vertices = vertices;
        adjacencyList = new ArrayList<>();
        for (int i = 0; i < vertices; i++) {
            adjacencyList.add(new ArrayList<>());
        }
    }

    // Method to add an edge to the graph
    void addEdge(int source, int destination, int weight) {
        adjacencyList.get(source).add(new Node(destination, weight));
        adjacencyList.get(destination).add(new Node(source, weight)); // For undirected graph
    }

    // Method to implement Dijkstra's algorithm
    void dijkstra(int source) {
        int[] distances = new int[vertices]; // Array to store the shortest distance from source
        Arrays.fill(distances, Integer.MAX_VALUE); // Initialize all distances to infinity
        distances[source] = 0; // Distance to source is 0

        PriorityQueue<Node> priorityQueue = new PriorityQueue<>(); // Min-heap priority queue
        priorityQueue.add(new Node(source, 0)); // Add source node to priority queue

        while (!priorityQueue.isEmpty()) {
            Node currentNode = priorityQueue.poll();
            int currentVertex = currentNode.vertex;

            // Iterate over adjacent nodes of the current node
            for (Node neighbor : adjacencyList.get(currentVertex)) {
                int newDist = distances[currentVertex] + neighbor.weight;

                // If a shorter path is found
                if (newDist < distances[neighbor.vertex]) {
                    distances[neighbor.vertex] = newDist;
                    priorityQueue.add(new Node(neighbor.vertex, newDist)); // Add updated node to queue
                }
            }
        }

        // Print the shortest distances from source to all other vertices
        System.out.println("Vertex\tDistance from Source");
        for (int i = 0; i < vertices; i++) {
            System.out.println(i + "\t\t" + distances[i]);
        }
    }

    public static void main(String[] args) {
        Graph graph = new Graph(6);
        graph.addEdge(0, 1, 4);
        graph.addEdge(0, 2, 1);
        graph.addEdge(1, 3, 1);
        graph.addEdge(2, 1, 2);
        graph.addEdge(2, 3, 5);
        graph.addEdge(3, 4, 3);
        graph.addEdge(4, 5, 1);

        graph.dijkstra(0); // Run Dijkstra's algorithm starting from vertex 0
    }
}
