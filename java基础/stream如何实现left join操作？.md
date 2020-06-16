```
作者：king-yu
链接：https://www.zhihu.com/question/345344979/answer/1010451159
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

class Join {
    @Data
    @AllArgsConstructor
    private static class Person {
        private Integer id;
        private String name;
        private Integer age;

    }

    @Data
    @AllArgsConstructor
    private static class Name {
        private Integer id;
        private String name;
    }

    @Data
    @AllArgsConstructor
    private static class Age {
        private Integer id;
        private Integer age;
    }



    public static void main(String[] args) {
        Name[] names = new Name[]{new Name(1, "name1"), new Name(1, "name2"), new Name(2, "name3")};
        Age[] ages = new Age[]{new Age(1, 18), new Age(2, 20)};

        //将第二个集合转换为map
        Map<Integer, Integer> ageMap = Arrays.stream(ages).collect(Collectors.toMap(Age::getId, Age::getAge));

        //遍历第一个集合，跟第二个的对应
        List<Person> personList = Arrays.stream(names).map(name -> new Person(name.getId(), name.getName(), ageMap.get(name.getId()))).collect(Collectors.toList());
        System.out.println(personList);
    }
}
```

