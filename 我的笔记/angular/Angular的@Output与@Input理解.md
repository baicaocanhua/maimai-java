 https://blog.csdn.net/wf19930209/article/details/79349164 

# Angular的@Output与@Input理解



# Angular @Output输出事件详解

 https://blog.csdn.net/qq_41248529/article/details/83865253 



# [Angular4学习笔记（六）- Input和Output](https://www.cnblogs.com/yw0219/p/7768960.html)

@input 父向子传递



```
parnet
data = [1,2,3];
<app-child [values]="data" >

</app-child>
```







```
child

@Input() values;

<p *ngFor="let item of values; let i = index">
  {{item}}
</p>
```





```
父
xiaomaiValue = 'duoduoduoduo';

<app-simple-form  (customClick)="onCustomClicked($event)" [xiaomai]="xiaomaiValue"></app-simple-form>
子
@Input() xiaomai: string;

<div>{{xiaomai}}</div>
```



@output 子向父传递

```
子
@Output() customClick = new EventEmitter<number>();

 onClicked() {
    console.log('2223333');
    alert('2222');
    this.customClick.emit(99);
  }


  <div (click)="onClicked()">
  11111111111 
  11111111
  <button>Click</button>
</div>

父
  showMsg = 1;

  onCustomClicked(num: number) {
    this.showMsg = num;
  }

  <p>@output: {{showMsg}}</p>

  <app-simple-form  (customClick)="onCustomClicked($event)" [xiaomai]="xiaomaiValue"></app-simple-form>
```

```
子
@Output() childEvent = new EventEmitter<any>();

  fireChildEvent(index) {
    this.childEvent.emit(index);
  }


  <p *ngFor="let item of values; let i = index" (click)="fireChildEvent(i)">
  {{item}}
</p>
 
父
getChildEvent(index){
    console.log(index);
    this.data.splice(index,1);
  }

  <app-child [values]="data" (childEvent) = "getChildEvent($event)">
</app-child>
```

