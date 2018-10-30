#队列 先进先出

``` typescript

export class Queue<T> {

    private elements: Array<T>;
    private _size: number | undefined;

    public constructor(capacity?: number) {
        this.elements = new Array<T>();
        this._size = capacity;
    }

    public push(o: T) {
        if (o == null) {
            return false;
        }
        //如果传递了size参数就设置了队列的大小
        if (this._size != undefined && !isNaN(this._size)) {
            if (this.elements.length == this._size) {
                this.pop();
            }
        }
        this.elements.unshift(o);
        return true;
    }

    public pop(): T {
        return this.elements.pop();
    }

    public size(): number {
        return this.elements.length;
    }

    public empty(): boolean {
        return this.size() == 0;
    }

    public clear() {
        delete this.elements;
        this.elements = new Array<T>();
    }
}

```

# 栈
先进后出, 同时Array自带push和pop方法，直接就可以当做栈使用了

``` typescript
const CAPACITY:number=10;

export class Stack<T> {
        
        private elements:Array<T>;
        private _size:number;
        
        public constructor(capacity:number = CAPACITY){
            this.elements = new Array<T>(capacity);
            this._size = 0;
        }

        public push(o:T){
            var len = this.elements.length;
            if(this._size >= len){
                let temp = new Array<T>(len);
                this.elements=this.elements.concat(temp);
            }
            this.elements[this._size++]=o;
        }

        public pop():T{
            return this.elements[--this._size];
        }

        public peek():T{
            return this.elements[this._size-1];
        }

        public size():number{
            return this._size;
        }
        
        public empty():boolean{
            return this._size==0;
        }

        public clear(capacity:number = CAPACITY){
            delete this.elements;
            this.elements = new Array(capacity);
            this._size = 0;
        }
}
```

# 集合

``` typescript
export interface Set<T>{
        add(t:T);
        remove(t:T);
        indexOf(t:T):number;
        size():number;
        clear();
        toArray():T[];
}

export class ArraySet<T> implements Set<T>{
        private arr:Array<T> = [];

        public add(t:T){
            this.indexOf(t) < 0 && this.arr.push(t);
        }

        public remove(t:T){
            var i = this.indexOf(t);
            if(i >= 0){
                delete this.arr[i];
            }
        }

        public indexOf(t:T):number{
            return this.arr.indexOf(t);
        }

        public size():number{
            return Object.keys(this.arr).length;
        }

        public clear(){
            delete this.arr;
            this.arr = [];
        }

        public toArray():T[]{
            var arr = new Array<T>();
            for(var i = 0; i < this.arr.length; i ++){
                this.arr[i] && arr.push(this.arr[i]);
            }
            return arr;
        }
}
```



[https://www.jianshu.com/p/82006ffb5bb7](https://www.jianshu.com/p/82006ffb5bb7)