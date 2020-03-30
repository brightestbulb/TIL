```java
import java.util.Stack;

public class QStack {
    Stack<Integer> oldStack;
    Stack<Integer> newStack;

    public QStack() {
        this.oldStack = new Stack<Integer>();
        this.newStack = new Stack<Integer>();
    }

    public void add(int num){
        newStack.push(num);
    }

    public int peek(){
        if(oldStack.empty()) {
            while(!newStack.empty()) {
                oldStack.push(newStack.pop());
            }
        }
        return oldStack.peek();
    }

    public int poll(){
        if(oldStack.empty()) {
            while(!newStack.empty()) {
                oldStack.push(newStack.pop());
            }
        }
        return oldStack.pop();
    }
}
```