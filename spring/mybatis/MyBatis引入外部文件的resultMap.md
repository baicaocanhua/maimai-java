一.使用 
1.有resultMap属性的标签都可以使用

> <select resultMap="命名空间.resultMap的id"></select>
> <association resultMap="命名空间.resultMap的id"></association>
> <collection resultMap="命名空间.resultMap的id"></collection>

2.某些标签的extends熟悉应该也能使用(猜测的，待验证)

> <resultMap extends="命名空间.resultMap的id"></resultMap>

二.格式

> 命名空间.resultMap的id