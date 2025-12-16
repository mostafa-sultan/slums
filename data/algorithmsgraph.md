# Graph Algorithms

## Introduction

Graph algorithms solve problems on graphs. This tutorial covers BFS, DFS, and shortest path algorithms.

---

## BFS

    function bfs(graph, start) {
      const queue = [start];
      const visited = new Set();
      while (queue.length > 0) {
        const node = queue.shift();
        if (!visited.has(node)) {
          visited.add(node);
          queue.push(...graph[node]);
        }
      }
      return visited;
    }

---

## Conclusion

Use BFS for shortest path problems and DFS for exploring all nodes. Choose based on your specific requirements.

