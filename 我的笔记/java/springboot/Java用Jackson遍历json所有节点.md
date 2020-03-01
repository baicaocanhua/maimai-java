# [Java用Jackson遍历json所有节点](https://www.cnblogs.com/witpool/p/8444700.html)




  

        public static void jsonLeaf(JsonNode node)
        {
            if (node.isValueNode())
            {
                System.out.println(node.toString());
                return;
            }
        if (node.isObject())
        {
            Iterator<Entry<String, JsonNode>> it = node.fields();
            while (it.hasNext())
            {
                Entry<String, JsonNode> entry = it.next();
                jsonLeaf(entry.getValue());
            }
        }
    
        if (node.isArray())
        {
            Iterator<JsonNode> it = node.iterator();
            while (it.hasNext())
            {
                jsonLeaf(it.next());
            }
        }
    }
    
    public static void main(String[] args)
    {
        try
        {
            String json = FileUtils.readFileToString(new File("C://test.json"), "UTF-8");
            ObjectMapper jackson = new ObjectMapper();
            JsonNode node = jackson.readTree(txt);
            jsonLeaf(node);
        }
        catch(Exception e)
        {
            e.printStackTrace();
        }
    }