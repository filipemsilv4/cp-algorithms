---
tags:
  - Translated
e_maxx_link: bfs
---

# Busca em Largura (BFS)

A busca em largura (BFS - Breadth First Search) é um dos algoritmos de busca mais básicos e essenciais em grafos.

Como resultado de seu funcionamento, o caminho encontrado pela BFS para qualquer nó é o caminho mais curto para aquele nó, ou seja, o caminho que contém o menor número de arestas em grafos não ponderados.

O algoritmo funciona em tempo $O(n + m)$, onde $n$ é o número de vértices e $m$ é o número de arestas.

## Descrição do Algoritmo

O algoritmo recebe como entrada um grafo não ponderado e o ID do vértice de origem $s$. O grafo de entrada pode ser direcionado ou não direcionado, isso não importa para o algoritmo.

O algoritmo pode ser entendido como um incêndio se espalhando pelo grafo: na etapa zero, apenas a origem $s$ está em chamas. A cada etapa, o fogo queimando em cada vértice se espalha para todos os seus vizinhos. Em uma iteração do algoritmo, o "anel de fogo" se expande em largura por uma unidade (daí o nome do algoritmo).

Mais precisamente, o algoritmo pode ser descrito da seguinte forma: Crie uma fila $q$ que conterá os vértices a serem processados e um array booleano $used[]$ que indica, para cada vértice, se ele já foi queimado (ou visitado) ou não.

Inicialmente, insira a origem $s$ na fila e defina $used[s] = true$ e, para todos os outros vértices $v$, defina $used[v] = false$. Em seguida, execute um loop até que a fila esteja vazia e, em cada iteração, remova um vértice da frente da fila. Itere por todas as arestas que saem deste vértice e, se alguma dessas arestas for para vértices que ainda não foram queimados, queime-os e coloque-os na fila.

Como resultado, quando a fila estiver vazia, o "anel de fogo" conterá todos os vértices alcançáveis a partir da origem $s$, com cada vértice alcançado da maneira mais curta possível. Você também pode calcular os comprimentos dos caminhos mais curtos (o que requer apenas a manutenção de um array de comprimentos de caminho $d[]$), bem como salvar informações para recuperar todos esses caminhos mais curtos (para isso, é necessário manter um array de "pais" $p[]$, que armazena para cada vértice o vértice a partir do qual o alcançamos).

## Implementação

Escrevemos o código para o algoritmo descrito em C++ e Java.

=== "C++"
    ```cpp
    vector<vector<int>> adj;  // Representação da lista de adjacência
    int n; // Número de nós
    int s; // Vértice de origem

    queue<int> q;
    vector<bool> used(n);
    vector<int> d(n), p(n);

    q.push(s);
    used[s] = true;
    p[s] = -1;
    while (!q.empty()) {
        int v = q.front();
        q.pop();
        for (int u : adj[v]) {
            if (!used[u]) {
                used[u] = true;
                q.push(u);
                d[u] = d[v] + 1;
                p[u] = v;
            }
        }
    }
    ```
=== "Java"
    ```java
    ArrayList<ArrayList<Integer>> adj = new ArrayList<>(); // Representação da lista de adjacência
        
    int n; // Número de nós
    int s; // Vértice de origem


    LinkedList<Integer> q = new LinkedList<Integer>();
    boolean used[] = new boolean[n];
    int d[] = new int[n];
    int p[] = new int[n];

    q.push(s);
    used[s] = true;
    p[s] = -1;
    while (!q.isEmpty()) {
        int v = q.pop();
        for (int u : adj.get(v)) {
            if (!used[u]) {
                used[u] = true;
                q.push(u);
                d[u] = d[v] + 1;
                p[u] = v;
            }
        }
    }
    ```
    
Se precisarmos recuperar e exibir o caminho mais curto da origem para algum vértice $u$, isso pode ser feito da seguinte maneira:
    
=== "C++"
    ```cpp
    if (!used[u]) {
        cout << "Sem caminho!";
    } else {
        vector<int> path;
        for (int v = u; v != -1; v = p[v])
            path.push_back(v);
        reverse(path.begin(), path.end());
        cout << "Caminho: ";
        for (int v : path)
            cout << v << " ";
    }
    ```
=== "Java"
    ```java
    if (!used[u]) {
        System.out.println("Sem caminho!");
    } else {
        ArrayList<Integer> path = new ArrayList<Integer>();
        for (int v = u; v != -1; v = p[v])
            path.add(v);
        Collections.reverse(path);
        for(int v : path)
            System.out.println(v);
    }
    ```
    
## Aplicações da BFS

* Encontrar o caminho mais curto de uma origem para outros vértices em um grafo não ponderado.

* Encontrar todos os componentes conexos em um grafo não direcionado em tempo $O(n + m)$:
Para fazer isso, basta executar a BFS a partir de cada vértice, exceto para vértices que já foram visitados em execuções anteriores. Assim, realizamos a BFS normal a partir de cada um dos vértices, mas não redefinimos o array $used[]$ toda vez que obtemos um novo componente conectado, e o tempo total de execução ainda será $O(n + m)$ (executar múltiplas BFS no grafo sem zerar o array $used[]$ é chamado de série de buscas em largura).

* Encontrar uma solução para um problema ou jogo com o menor número de movimentos, se cada estado do jogo puder ser representado por um vértice do grafo e as transições de um estado para o outro forem as arestas do grafo.

* Encontrar o caminho mais curto em um grafo com pesos 0 ou 1:
Isso requer apenas uma pequena modificação na busca em largura normal: em vez de manter o array $used[]$, agora verificaremos se a distância até o vértice é menor que a distância encontrada atualmente; então, se a aresta atual tiver peso zero, nós a adicionamos à frente da fila; caso contrário, nós a adicionamos ao final da fila. Esta modificação é explicada com mais detalhes no artigo [0-1 BFS](01_bfs.md).

* Encontrar o menor ciclo em um grafo direcionado não ponderado:
Inicie uma busca em largura a partir de cada vértice. Assim que tentarmos voltar do vértice atual para o vértice de origem, teremos encontrado o menor ciclo contendo o vértice de origem. Neste ponto, podemos parar a BFS e iniciar uma nova BFS a partir do próximo vértice. De todos esses ciclos (no máximo um de cada BFS), escolha o mais curto.

* Encontrar todas as arestas que estão em qualquer caminho mais curto entre um determinado par de vértices $(a, b)$.
Para fazer isso, execute duas buscas em largura: uma de $a$ e outra de $b$. Seja $d_a []$ o array contendo as menores distâncias obtidas da primeira BFS (de $a$) e $d_b []$ o array contendo as menores distâncias obtidas da segunda BFS (de $b$). Agora, para cada aresta $(u, v)$, é fácil verificar se essa aresta está em qualquer caminho mais curto entre $a$ e $b$: o critério é a condição $d_a [u] + 1 + d_b [v] = d_a [b]$.

* Encontrar todos os vértices em qualquer caminho mais curto entre um determinado par de vértices $(a, b)$.
Para fazer isso, execute duas buscas em largura: uma de $a$ e outra de $b$. Seja $d_a []$ o array contendo as menores distâncias obtidas da primeira BFS (de $a$) e $d_b []$ o array contendo as menores distâncias obtidas da segunda BFS (de $b$). Agora, para cada vértice, é fácil verificar se ele está em qualquer caminho mais curto entre $a$ e $b$: o critério é a condição $d_a [v] + d_b [v] = d_a [b]$.

* Encontrar o caminho mais curto de comprimento par de um vértice de origem $s$ para um vértice de destino $t$ em um grafo não ponderado:
Para isso, devemos construir um grafo auxiliar, cujos vértices são o estado $(v, c)$, onde $v$ é o nó atual e $c = 0$ ou $c = 1$ é a paridade atual. Qualquer aresta $(u, v)$ do grafo original nesta nova coluna se transformará em duas arestas $((u, 0), (v, 1))$ e $((u, 1), (v, 0))$. Depois disso, executamos uma BFS para encontrar o caminho mais curto do vértice inicial $(s, 0)$ ao vértice final $(t, 0)$.


## Problemas Práticos

* [SPOJ: AKBAR](http://spoj.com/problems/AKBAR)
* [SPOJ: NAKANJ](http://www.spoj.com/problems/NAKANJ/)
* [SPOJ: WATER](http://www.spoj.com/problems/WATER)
* [SPOJ: MICE AND MAZE](http://www.spoj.com/problems/MICEMAZE/)
* [Timus: Caravans](http://acm.timus.ru/problem.aspx?space=1&num=2034)
* [DevSkill - Holloween Party (arquivado)](http://web.archive.org/web/20200930162803/http://www.devskill.com/CodingProblems/ViewProblem/60)
* [DevSkill - Ohani And The Link Cut Tree (arquivado)](http://web.archive.org/web/20170216192002/http://devskill.com:80/CodingProblems/ViewProblem/150)
* [SPOJ - Spiky Mazes](http://www.spoj.com/problems/SPIKES/)
* [SPOJ - Four Chips (difícil)](http://www.spoj.com/problems/ADV04F1/)
* [SPOJ - Inversion Sort](http://www.spoj.com/problems/INVESORT/)
* [Codeforces - Shortest Path](http://codeforces.com/contest/59/problem/E)
* [SPOJ - Yet Another Multiple Problem](http://www.spoj.com/problems/MULTII/)
* [UVA 11392 - Binary 3xType Multiple](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2387)
* [UVA 10968 - KuPellaKeS](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1909)
* [Codeforces - Police Stations](http://codeforces.com/contest/796/problem/D)
* [Codeforces - Okabe and City](http://codeforces.com/contest/821/problem/D)
* [SPOJ - Find the Treasure](http://www.spoj.com/problems/DIGOKEYS/)
* [Codeforces - Bear and Forgotten Tree 2](http://codeforces.com/contest/653/problem/E)
* [Codeforces - Cycle in Maze](http://codeforces.com/contest/769/problem/C)
* [UVA - 11312 - Flipping Frustration](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2287)
* [SPOJ - Ada and Cycle](http://www.spoj.com/problems/ADACYCLE/)
* [CSES - Labyrinth](https://cses.fi/problemset/task/1193)
* [CSES - Message Route](https://cses.fi/problemset/task/1667/)
* [CSES - Monsters](https://cses.fi/problemset/task/1194)
