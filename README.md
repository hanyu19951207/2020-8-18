# 2020-8-18
49. 字母异位词分组
    给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
    示例:
    输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
    输出:
    [
      ["ate","eat","tea"],
      ["nat","tan"],
      ["bat"]
    ]
思路：
  当且仅当它们的排序字符串相等时，两个字符串是字母异位词。
  维护一个映射 ans : {String -> List}，其中每个键 K 是一个排序字符串，每个值是初始输入的字符串列表，排序后等于 K。
题解：
 class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs.length == 0)
            return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        for(String s : strs){
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if(!ans.containsKey(key))
                ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }   
}
581. 最短无序连续子数组
    给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
    你找到的子数组应是最短的，请输出它的长度。
    示例 1:
    输入: [2, 6, 4, 8, 10, 9, 15]
    输出: 5
    解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
思路：
    我们将数组 numsnums 进行排序，记为 nums_sorted。然后我们比较 nums 和 nums_sorted 的元素来决定最左边和最右边不匹配的元素。
    它们之间的子数组就是要求的最短无序子数组。
题解：
  public class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] snums = nums.clone();
        Arrays.sort(snums);
        int start = snums.length, end = 0;
        for (int i = 0; i < snums.length; i++) {
            if (snums[i] != nums[i]) {
                start = Math.min(start, i);
                end = Math.max(end, i);
            }
        }
        return (end - start >= 0 ? end - start + 1 : 0);
    }
}
589. N叉树的前序遍历
    给定一个 N 叉树，返回其节点值的前序遍历。
思路：
    层序遍历借助队列，初始时将根结点入列，此后只要队列不空，就出列一个结点，并将它的子结点从左到右依次入列。
    这种方式每次从队首出列的结点可能是某个结点的左子节点，也可能是某个结点的右子节点。
    如何由此向前序遍历的方向靠呢？
    再看一下队尾，如果结点每次都从队尾出列，然后再将子结点从左到右入列，很容易发现，这是一种根->右->左的前序遍历。
    那么此时已经茅塞顿开，如果把入列方式改为将子结点从右到左入列，再每次都从队尾出列，这就是根->左->右的前序遍历。
    此外，若每次从队尾出列，属于后进先出，相当于栈，则可用栈代替队列。
题解：
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> res = new LinkedList<>();
        if(root == null)    return res;
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            Node cur = stack.pop();
            res.add(cur.val);
            for(int i = cur.children.size() - 1;i >= 0;i--)
                stack.push(cur.children.get(i));
        } 
        return res;
    }
}
