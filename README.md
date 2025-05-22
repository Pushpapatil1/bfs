!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Colorful Graph Picture - BFS Traversal</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
            background: #f0f8ff;
        }
        input, button {
            padding: 8px;
            margin: 5px;
            border-radius: 5px;
            border: 1px solid #888;
        }
        button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        canvas {
            border: 2px solid #ccc;
            margin-top: 20px;
            background: #ffffff;
            box-shadow: 0 0 10px #aaa;
        }
        #output, #bfsTheory, #graphSection {
            margin-top: 30px;
            font-size: 18px;
            text-align: left;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
            background: #ffffff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px #aaa;
        }
        #bfsTheory h2 {
            text-align: center;
            color: #333;
        }
        pre {
            background: #f4f4f4;
            padding: 10px;
            border-radius: 8px;
            overflow-x: auto;
        }
        #nextButton {
            margin-top: 20px;
        }
    </style>
</head>
<body>

<h1>Breadth-First Search (BFS) Explanation</h1>

<!-- BFS THEORY SECTION FIRST -->
<div id="bfsTheory">
    <h2>Breadth-First Search (BFS)</h2>

    <h3>Definition:</h3>
    <p>
        Breadth-First Search (BFS) is a fundamental graph traversal algorithm that explores all the vertices of a graph or tree in breadthward motion (i.e., it visits all the neighbors at the current depth prior to moving on to nodes at the next depth level).  
        <br><br>
        In BFS, starting from a selected node (called the "source" node), the algorithm visits all nodes reachable from it by systematically exploring the neighbor nodes before moving to the next level of neighbors.
    </p>

    <h3>Steps of BFS:</h3>
    <ol>
        <li>Initialize a queue and enqueue the start node.</li>
        <li>Mark the start node as visited.</li>
        <li>While the queue is not empty:
            <ul>
                <li>Dequeue a node from the queue.</li>
                <li>Process the current node (e.g., print it or store it in traversal order).</li>
                <li>For each unvisited neighbor of the current node:
                    <ul>
                        <li>Mark the neighbor as visited.</li>
                        <li>Enqueue the neighbor.</li>
                    </ul>
                </li>
            </ul>
        </li>
    </ol>

    <h3>Algorithm (Pseudocode):</h3>
    <pre>
BFS(Graph, start vertex):

    Initialize an empty queue Q
    Initialize a set visited to keep track of visited vertices

    Enqueue start vertex into Q
    Add start vertex to visited

    while Q is not empty do
        v = Q.dequeue()
        process v

        for each neighbor u of v do
            if u is not in visited then
                add u to visited
                enqueue u into Q
    </pre>

    <h3>Time Complexity:</h3>
    <p><strong>O(V + E)</strong>, where:
        <ul>
            <li><strong>V</strong> is the number of vertices (nodes)</li>
            <li><strong>E</strong> is the number of edges</li>
        </ul>
    </p>

    <div style="text-align:center;">
        <button id="nextButton" onclick="showGraphSection()">Next</button>
    </div>
</div>

<!-- HIDDEN GRAPH SECTION -->
<div id="graphSection" style="display:none;">
    <h1>BFS Traversal</h1>

    <div>
        <input type="text" id="nodes" placeholder="Nodes (comma separated)">
        <input type="text" id="edges" placeholder="Edges (e.g., A-B,B-C)">
        <button onclick="createGraph()">Create Graph</button>
    </div>

    <div id="startNodeDiv" style="display:none;">
        <input type="text" id="startNode" placeholder="Start Node for BFS">
        <button onclick="startBFS()">Start BFS</button>
    </div>

    <canvas id="graphCanvas" width="800" height="600"></canvas>

    <div id="output"></div>
</div>

<script>
    function showGraphSection() {
        document.getElementById('bfsTheory').style.display = 'none';
        document.getElementById('graphSection').style.display = 'block';
    }

    let graph = {};
    let nodePositions = {};
    let nodeColors = {};

    const canvas = document.getElementById('graphCanvas');
    const ctx = canvas.getContext('2d');

    function randomPosition() {
        return {
            x: Math.random() * 700 + 50,
            y: Math.random() * 500 + 50
        };
    }

    function randomColor() {
        const colors = ['#FF6B6B', '#6BCB77', '#4D96FF', '#FFD93D', '#FF6F91', '#845EC2', '#00C9A7', '#FFC75F'];
        return colors[Math.floor(Math.random() * colors.length)];
    }

    function createGraph() {
        const nodesInput = document.getElementById('nodes').value.trim();
        const edgesInput = document.getElementById('edges').value.trim();

        const nodes = nodesInput.split(',').map(n => n.trim());
        const edges = edgesInput.split(',').map(e => e.trim());

        graph = {};
        nodePositions = {};
        nodeColors = {};

        nodes.forEach(node => {
            graph[node] = [];
            nodePositions[node] = randomPosition();
            nodeColors[node] = randomColor();
        });

        edges.forEach(edge => {
            const [from, to] = edge.split('-');
            if (graph[from] && graph[to]) {
                graph[from].push(to);
                graph[to].push(from);
            }
        });

        drawGraph();
        document.getElementById('startNodeDiv').style.display = 'block';
    }

    function drawGraph() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        for (let node in graph) {
            graph[node].forEach(neighbor => {
                ctx.beginPath();
                ctx.strokeStyle = '#aaa';
                ctx.lineWidth = 2;
                ctx.moveTo(nodePositions[node].x, nodePositions[node].y);
                ctx.lineTo(nodePositions[neighbor].x, nodePositions[neighbor].y);
                ctx.stroke();
            });
        }

        for (let node in nodePositions) {
            ctx.beginPath();
            ctx.arc(nodePositions[node].x, nodePositions[node].y, 20, 0, Math.PI * 2);
            ctx.fillStyle = nodeColors[node];
            ctx.fill();
            ctx.strokeStyle = '#333';
            ctx.lineWidth = 2;
            ctx.stroke();

            ctx.fillStyle = 'white';
            ctx.font = '16px Arial';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(node, nodePositions[node].x, nodePositions[node].y);
        }
    }

    function startBFS() {
        const start = document.getElementById('startNode').value.trim();
        if (!graph[start]) {
            alert('Start node not found in the graph!');
            return;
        }

        let visited = new Set();
        let queue = [];
        let order = [];

        queue.push(start);

        const bfsInterval = setInterval(() => {
            if (queue.length === 0) {
                clearInterval(bfsInterval);

                const V = Object.keys(graph).length;
                let E = 0;
                for (let node in graph) {
                    E += graph[node].length;
                }
                E = E / 2;

                document.getElementById('output').innerHTML = `
                    <p><strong>BFS Order:</strong> ${order.join(' → ')}</p>
                    <p><strong>Time Complexity:</strong> O(V + E)</p>
                    <p><strong>Where</strong> V = ${V}, E = ${E}</p>
                `;
                return;
            }

            const node = queue.shift();
            if (!visited.has(node)) {
                visited.add(node);
                order.push(node);

                ctx.beginPath();
                ctx.arc(nodePositions[node].x, nodePositions[node].y, 20, 0, Math.PI * 2);
                ctx.fillStyle = '#00C9A7';
                ctx.fill();
                ctx.strokeStyle = '#333';
                ctx.lineWidth = 2;
                ctx.stroke();

                ctx.fillStyle = 'white';
                ctx.font = '16px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(node, nodePositions[node].x, nodePositions[node].y);

                graph[node].forEach(neighbor => {
                    if (!visited.has(neighbor)) {
                        queue.push(neighbor);
                    }
                });
            }
        }, 700);
    }
</script>

</body>
</html>
