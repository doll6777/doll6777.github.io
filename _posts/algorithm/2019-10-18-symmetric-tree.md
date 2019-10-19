---
layout: post
title: (Tree) Symmetric Tree
tags: [algorithm, programming, leetcode]
category : algorithm
---

이진 트리가 주어질 때, 노드의 값들이 좌우 대칭으로 이루어져 있는지 판단하는 알고리즘을 작성하라. 

<pre class="prettyprint">
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

class Solution {

    private boolean isSymmetric(TreeNode left, TreeNode right) {
        if(left == null && right == null) {
            return true;
        }
        if(left == null) {
            left = new TreeNode(-1);
        }
        if(right == null) {
            right = new TreeNode(-1);
        }

        if(left.val != right.val) {
            return false;
        }

        boolean leftResult = isSymmetric(left.left, right.right);
        boolean rightResult = isSymmetric(left.right, right.left);
        
        return leftResult && rightResult;
    }

    public boolean isSymmetric(TreeNode root) {
        if(root == null) {
            return true;
        }
        return isSymmetric(root.left, root.right);
    }
}
</pre>

재귀 함수를 사용한다. 트리를 탐색하는 데 동시에 두 서브트리를 탐색한다. 
preorder와 postorder를 동시에 사용하고 해당 값이 같지 않으면 탐색을 즉시 종료한다.  

리턴 조건은 
- 값이 같지 않을 때 ( return false )
- 자식이 모두 null일때 ( return true ) : 이때는 더이상 깊게 들어갈 필요는 없지만 이전 노드로 돌아갈 필요는 있다.

주의해야 할 점은
- 자식 중 하나가 null인 경우 : NullPointerException이 발생할 수 있으니 value를 참조할 때 예외처리를 넣어준다. 나는 그냥 -1을 담은 노드를 생성해 넣어주었다. 이는 트리에 0 이상만 들어온다는 가정 하에 가능하다.
- 트리가 null로 들어올 경우

이를 더 간략화하면
<pre class="prettyprint">
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

public boolean isMirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.val == t2.val)
        && isMirror(t1.right, t2.left)
        && isMirror(t1.left, t2.right);
}
</pre>

또 다른 방법으로는, BFS로 푸는 방법이 존재한다.
<pre class="prettyprint">
public boolean isSymmetric(TreeNode root) {
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode t1 = q.poll();
        TreeNode t2 = q.poll();
        if (t1 == null && t2 == null) continue;
        if (t1 == null || t2 == null) return false;
        if (t1.val != t2.val) return false;
        q.add(t1.left);
        q.add(t2.right);
        q.add(t1.right);
        q.add(t2.left);
    }
    return true;
}
</pre>
