stream  map 返回部分属性的数据
 List<Map<String, Object>> dishNames = menu.stream()
                .map(key -> {
                    String name = key.getName();
                    boolean vegetarian = key.isVegetarian();
                    Map<String, Object> goodObject = new HashMap<>(8);
                    goodObject.put("name", name);
                    goodObject.put("vegetarian", vegetarian);
                    return goodObject;
                })
                .collect(Collectors.toList());
        for (Map<String, Object> dishName : dishNames) {
            for (String key : dishName.keySet()) {
                System.out.println(key + "=" + dishName.get(key));
            }
        }