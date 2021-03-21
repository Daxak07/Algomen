# SHORTEST PATH by Algomen

## REAL-LIFE GRAPHS
*Graph theory is the study of graphs, which are mathematical structures used to model pairwise relations between objects. A graph in this context is made up of vertices (also called nodes or points) which are connected by edges.*
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/graph.png" width="500">
</br>
**EVERYTHING IN OUR WORLD IS LINKED:**
* All cities are linked by roads
* Pages are linked by hyperlinks on the internet
* Flight and rail network
* Mobile network
* Components of Electric circuit
* Components of computer chips and etc…

*By graphs, we can simulate all these networks to make some visual analysis like finding connections and shortest paths between nodes.*

## Algorithm

**What is Dijkstra’s shortest path algorithm?**

* Dijkstra algorithm is a greedy algorithm.
* It finds a shortest path tree for a weighted undirected graph.
* This means it finds a shortest paths between nodes in a graph, which may represent, for example, road networks
* For a given source node in the graph, the algorithm finds the shortest path between source node and every other node.
* This algorithm also used for finding the shortest paths from a single node to a single destination node by stopping the algorithm once the shortest path to the destination node has been determined.
* Dijkstra’s algorithm is very similar to Prim’s algorithm. In Prim’s algorithm we create minimum spanning tree (MST) and in Dijkstra algorithm we create shortest path tree (SPT) from the given source...

## Shortest path Applications

### Games 
What about uses of the shortest path algorithms, the very first thing that came to our mind was the project we worked on in the java2 course. 
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/3edited.gif" width="500">

### Map services 
Shortest path algorithms are applied to automatically find directions between physical locations, such as driving directions on web mapping websites like MapQuest or Google Maps.
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/1fasted.gif" width="500">

*Analysis and results from gif:*
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/analysis.png" width="350">
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/result.png" width="350">

### Nondeterministic machine
If one represents a nondeterministic abstract machine as a graph where vertices describe states and edges describe possible transitions, shortest path algorithms can be used to find an optimal sequence of choices to reach a certain goal state, or to establish lower bounds on the time needed to reach a given state.
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/nondeterministic.png" width="500">
</br>

*Real-life example:*
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/2fasted.gif" width="500">



## Code implementation
**Dijkstra's algorithm**
```
public class SPT {
    static class Graph {
        int vertices;
        int matrix[][];

        public Graph(int vertex) {
            this.vertices = vertex;
            matrix = new int[vertex][vertex];
        }

        public void addEdge(int v, int w, int c) {
            matrix[v][w] = c;
            matrix[w][v] = c;
        }


        int getMinVertex(boolean[] list, int[] key) {
            int minKey = Integer.MAX_VALUE;
            int vertex = -1;
            for (int i = 0; i < vertices; i++) {
                if (list[i] == false && minKey > key[i]) {
                    minKey = key[i];
                    vertex = i;
                }
            }
            return vertex;
        }

        public void getMinDist(int sourceVertex) {
            boolean[] spt = new boolean[vertices];
            int[] distance = new int[vertices];
            int INFINITY = Integer.MAX_VALUE;

            for (int i = 0; i < vertices; i++) {
                distance[i] = INFINITY;
            }

//            for (int i = 0; i < vertices; i++) {
//                for (int j = 0; j < vertices; j++) {
//                    System.out.print(matrix[i][j] + " ");
//                }
//                System.out.println();
//            }

            distance[sourceVertex] = 0;
            
            for (int i = 0; i < vertices; i++) {
                int vertex_U = getMinVertex(spt, distance);
//                System.out.println("Vertex from getMinVertex equals " + vertex_U);
                spt[vertex_U] = true;

//                System.out.print("[ ");
//                for (int j = 0; j < spt.length;j++) {
//                    System.out.print(spt[j] + " ");
//                }
//                System.out.println("]");

                for (int vertex_V = 0; vertex_V < vertices; vertex_V++) {
                    if (matrix[vertex_U][vertex_V] > 0 && spt[vertex_V] == false) {
//                        System.out.println("Matrix " + matrix[vertex_U][vertex_V]);
//                        System.out.println("Distance " + distance[vertex_U]);
                        int distanceUpdate = matrix[vertex_U][vertex_V] + distance[vertex_U];
//                        System.out.println("Sum of Matrix + Distance " + distanceUpdate);
                        if (distanceUpdate < distance[vertex_V])
                            distance[vertex_V] = distanceUpdate;
                    }
                }
            }
            //print shortest path tree
//            System.out.println( "Source Vertex " + sourceVertex);
//            for (int i = 0; i < distance.length;i++) {
//                System.out.println(i + " distance  " + distance[i]);
//            }
            printSPT(sourceVertex, distance);
        }

        public void printSPT(int sourceVertex, int[] key) {
            System.out.println("SPT Algorithm: ");
            for (int i = 0; i < vertices; i++) {
                System.out.println("Source Vertex: " + sourceVertex + " to vertex " + i +
                        " distance: " + key[i]);
            }
        }
    }

    public static void main(String[] args) {
        Graph graph = new Graph(6);
        int sourceVertex = 0;
        graph.addEdge(0, 1, 4);
        graph.addEdge(0, 2, 3);
        graph.addEdge(1, 2, 1);
        graph.addEdge(1, 3, 2);
        graph.addEdge(2, 3, 4);
        graph.addEdge(3, 4, 2);
        graph.addEdge(4, 5, 6);
        graph.getMinDist(sourceVertex);
    }
}
```
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/SPT.jpg" width="500">
</br>

**BellmanFord Algorithm:**

```
class SPT2 {
    static void BellmanFord(int graph[][], int V, int E, int src) {
        int []dis = new int[V];
        for (int i = 0; i < V; i++) {
            dis[i] = Integer.MAX_VALUE;
        }
     
        dis[src] = 0;
     
        for (int i = 0; i < V - 1; i++) 
        {
     
            for (int j = 0; j < E; j++) 
            {
                if (dis[graph[j][0]] != Integer.MAX_VALUE && dis[graph[j][0]] + graph[j][2] <
                                dis[graph[j][1]])
                    dis[graph[j][1]] = 
                    dis[graph[j][0]] + graph[j][2];
            }
        }
     

        for (int i = 0; i < E; i++) {
            int x = graph[i][0];
            int y = graph[i][1];
            int weight = graph[i][2];
            
            if (dis[x] != Integer.MAX_VALUE &&
                    dis[x] + weight < dis[y])
                System.out.println("Graph contains negative"
                        +" weight cycle");
        }
     
        System.out.println("Vertex Distance from Source");
        for (int i = 0; i < V; i++)
            System.out.println(i + "\t\t" + dis[i]);
    }
     
    public static void main(String[] args) {
        int V = 5; // Number of vertices in graph
        int E = 8; // Number of edges in graph

        int graph[][] = { { 0, 1, -1 }, { 0, 2, 4 },
                        { 1, 2, 3 }, { 1, 3, 2 }, 
                        { 1, 4, 2 }, { 3, 2, 5 }, 
                        { 3, 1, 1 }, { 4, 3, -3 } };
     
        BellmanFord(graph, V, E, 0);
    }
}
```
</br>
<img src="https://github.com/Daxak07/Algomen/blob/main/visuals/codeGraph.jpeg" width="500">
</br>