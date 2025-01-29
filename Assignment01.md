DSA Section:-------------------------------------------------------------

Q1:Leetcode 987 (Vertical Order Traversal of aBinary Tree)
Logic:
For vertical traversal of binary tree the  approach is to store the horizontal distance and level for each node , so that it become easy to track the nodes and for this i will be making  map ds which store hd and a
map with level and list of nodes , and another ds i.e queue for storing node, level and hd.

code:

class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        vector<vector<int>> result;
        map<int, map<int, vector<int>>> nodes;
        queue<pair<TreeNode*, pair<int, int>>> q;
        
        if (root == NULL) {
            return result;
        }
        
        q.push(make_pair(root, make_pair(0, 0)));
        
        while (!q.empty()) {
            pair<TreeNode*, pair<int, int>> temp = q.front();
            q.pop();
            
            TreeNode* frontNode = temp.first;
            int hd = temp.second.first;
            int lvl = temp.second.second;
            
            nodes[hd][lvl].push_back(frontNode->val);
            
            if (frontNode->left) {
                q.push(make_pair(frontNode->left, make_pair(hd - 1, lvl + 1)));
            }
            
            if (frontNode->right) {
                q.push(make_pair(frontNode->right, make_pair(hd + 1, lvl + 1)));
            }
        }
        
        for (auto i : nodes) {
            vector<int> column;
            for (auto j : i.second) {
                sort(j.second.begin(), j.second.end());
                for (int value : j.second) {
                    column.push_back(value);
                }
            }
            result.push_back(column);
        }
        
        return result;
    }
};


Q2:Leetcode 199 (Right Side view of Binary Tree)
Logic:
Just take first node from right side from each level, uses recursive traversing.

code:
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
    void traversal(TreeNode* Node, int level,vector<int>&ds){
        if(Node==NULL){
            return ;
        }
        if(level==ds.size()){
            ds.push_back(Node->val);

        }
        traversal(Node->right,level+1,ds);
        traversal(Node->left,level+1,ds);

    }
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int>ans(0);
         traversal(root, 0,ans);  
         return ans;     
    }
};

Q3:Leetcode 2381 (Shifting characters)
Logic:
Going with brute force will result into TLE so tried the concept of difference array to solve the problem in big(0) time complexity

code:
class Solution {
public:
    string shiftingLetters(string nums, vector<vector<int>>& shifts) {
        int n = nums.size();
        vector<int> diff(n + 1, 0);

        for (auto shift : shifts) {
            int s = shift[0];
            int e = shift[1];
            int d = shift[2] == 1 ? 1 : -1;
            diff[s] += d;
            diff[e + 1] -= d;
        }

        for (int i = 1; i < n; i++) {
            diff[i] += diff[i - 1];
        }

        for (int i = 0; i < n; i++) {
            if (diff[i] >= 0) {
                nums[i] = (nums[i] - 'a' + diff[i]) % 26 + 'a';
            } else {
                nums[i] = (nums[i] - 'a' + (diff[i] % 26) + 26) % 26 + 'a';
            }
        }

        return nums;
    }
};

DBMS SECTION:------------------------------------------------------

1757. Recyclable and Low Fat Products
Link: https://leetcode.com/problems/recyclable-and-low-fat-products/  description/?envType=study-plan-v2&envId=top-sql-50

Query:

select product_id from Products where low_fats = 'Y' and recyclable = 'Y';

OOPS SECTION:---------------------------------------------------------