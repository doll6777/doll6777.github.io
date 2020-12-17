---
layout: post
title: (Graph) Course Schedule
tags: [algorithm, programming, leetcode]
category : algorithm
---

수업 시간표를 짜는 문제. 그래프로 엣지들을 그려보면 사이클을 찾는 문제이다. DFS로 검사하며 사이클을 찾았다.

<pre class="prettyprint">
import java.util.ArrayList;
import java.util.List;

class Solution {
    List&lt;Edge&gt; visited = new ArrayList&lt;&gt;();

    class Edge {
        List&lt;Integer&gt; inList = new ArrayList&lt;&gt;();
        List&lt;Integer&gt; outList = new ArrayList&lt;&gt;();

        public void addIn(Integer in) {
            inList.add(in);
        }

        public void addOut(Integer out) {
            outList.add(out);
        }
    }

    // 4. cycle이 있는지 찾는다
    private boolean traverse(Edge[] edges, Edge edge, List&lt;Edge&gt; stack) {
        if (stack.contains(edge)) {
            return true;
        }

        if (!visited.contains(edge)) {
            visited.add(edge);
        } else {
            return false;
        }
        for (int i = 0; i &lt; edge.outList.size(); i++) {
            stack.add(edge);
            if (traverse(edges, edges[edge.outList.get(i)], stack)) {
                return true;
            }
            stack.remove(edge);
        }
        return false;
    }

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        boolean result = true;

        // 1. edge List를 만든다
        Edge[] edges = new Edge[numCourses + 1];

        if (prerequisites.length == 0 || prerequisites[0].length == 0) {
            return true;
        }

        // second -&gt; first
        for (int i = 0; i &lt; prerequisites.length; i++) {
            int first = prerequisites[i][0];
            int second = prerequisites[i][1];

            // NPE check
            if (edges[second] == null) {
                edges[second] = new Edge();
            }
            if (edges[first] == null) {
                edges[first] = new Edge();
            }
            edges[second].addOut(first);
            edges[first].addIn(first);
        }

        for (int i = 0; i &lt; edges.length; i++) {
            if (edges[i] == null) {
                continue;
            }
            // 3. leaf들을 dfs한다
            boolean isCycleDetected = traverse(edges, edges[i], new ArrayList&lt;&gt;());
            if (isCycleDetected) {
                result = false;
                break;
            }
        }
        return result;
    }
}
</pre>