



查询自己以及自己下面的所有



SELECT id,`name`,parentid FROM (
		SELECT *, IF(FIND_IN_SET(parentId,@pid)>0,@pid:=CONCAT(@pid,',',id),0) AS
		ischild FROM t_areainfo t1,(SELECT @pid:=1 ) a
		) AS tt WHERE ischild !=0





```java
package com.maimai.myspringboot.entity;

import java.util.ArrayList;
import java.util.List;
import com.alibaba.fastjson.JSONObject;

/** 
* @author YJJ
* @version 2018年11月2日 下午4:50:26
*/

public class TreeBuilder {

    List<Node> nodes = new ArrayList<>();

    public String buildTree(List<Node> nodes) {

        TreeBuilder treeBuilder = new TreeBuilder(nodes);

        return treeBuilder.buildJSONTree();
    }

    public TreeBuilder() {}

    public TreeBuilder(List<Node> nodes) {

        super();
        this.nodes = nodes;
    }

    // 构建JSON树形结构
    public String buildJSONTree() {

        List<Node> nodeTree = buildTree();


        String jsonString = JSONObject.toJSONString(nodeTree);


        return jsonString;
    }

    // 构建树形结构
    public List<Node> buildTree() {

        List<Node> treeNodes = new ArrayList<>();
        List<Node> rootNodes = getRootNodes();
        for (Node rootNode : rootNodes) {
            buildChildNodes(rootNode);
            treeNodes.add(rootNode);
        }
        return treeNodes;
    }

    // 递归子节点
    public void buildChildNodes(Node node) {

        List<Node> children = getChildNodes(node);
        if (!children.isEmpty()) {
            for (Node child : children) {

                buildChildNodes(child);
            }
            node.setChildren(children);
        }
    }

    // 获取父节点下所有的子节点
    public List<Node> getChildNodes(Node pnode) {

        List<Node> childNodes = new ArrayList<>();
        for (Node n : nodes) {
            if (pnode.getId().equals(n.getPid())) {
                childNodes.add(n);
            }
        }
        return childNodes;
    }

    // 判断是否为根节点
    public boolean rootNode(Node node) {

        boolean isRootNode = true;
        for (Node n : nodes) {
            if (node.getPid().equals(n.getId())) {
                isRootNode = false;
                break;
            }
        }
        return isRootNode;
    }

    // 获取集合中所有的根节点
    public List<Node> getRootNodes() {

        List<Node> rootNodes = new ArrayList<>();
        for (Node n : nodes) {
            if (rootNode(n)) {
                rootNodes.add(n);
            }
        }
        return rootNodes;
    }


}
```

```java
@RequestMapping(value = "/bulidJsonTree", method = RequestMethod.GET,
        produces = "application/json;charset=UTF-8")
public String buildJsonTree(HttpServletRequest request, HttpServletResponse response)
        throws Exception {
    //response.setContentType("application/json;charset=UTF-8");
    // 获取全部目录节点
    List<Node> nodes = treeService.getList();
    // 拼装树形json字符串
    String json = new TreeBuilder().buildTree(nodes);
    //writeJson(response, json);
    return json;
}
```



# mysql递归查询树形表

<https://blog.csdn.net/huweijun_2012/article/details/45133709>



# java+mysql递归拼接树形JSON列表

<https://blog.csdn.net/qq12547345/article/details/72765889>





# 将List转成树的两种方式(递归、循环)

<https://blog.csdn.net/massivestars/article/details/53911620/>





# 【Mysql】树路径，层级

<https://blog.csdn.net/AskTommorow/article/details/53084846>





# mysql 递归查询

<https://www.jianshu.com/p/d2e1d90b3d9a>





# [MySql 利用函数 查询所有子节点](https://www.cnblogs.com/nww57/p/5359113.html)





# MySQL 查询树结构

<https://blog.csdn.net/yangsen159/article/details/85164199>