出现在:阿里

使用三个线程轮流打印ABCABCABC

代码实现:
1,通过synchronized+wait()+notifyAll()实现
```
public class Main {
	private int flag = 0;

	public synchronized void printa() throws InterruptedException {
		while (true) {
			if (flag == 0) {
				System.out.println("A");
				flag = 1;
				notifyAll();
			}
			wait();
		}
	}

	public synchronized void printb() throws InterruptedException {
		while (true) {
			if (flag == 1) {
				System.out.println("B");
				flag = 2;
				notifyAll();
			}
			wait();
		}
	}

	public synchronized void printc() throws InterruptedException {
		while (true) {
			if (flag == 2) {
				System.out.println("C");
				flag = 0;
				notifyAll();
			}
			wait();
		}
	}

	public static void main(String[] args) {
		Main main = new Main();
		PrintA printA = new PrintA(main);
		PrintB printB = new PrintB(main);
		PrintC printC = new PrintC(main);
		Thread t1 = new Thread(printA);
		Thread t2 = new Thread(printB);
		Thread t3 = new Thread(printC);
		t1.start();
		t2.start();
		t3.start();
	}
}

class PrintA implements Runnable {
	private Main m;

	public PrintA(Main m) {
		this.m = m;
	}

	@Override
	public void run() {
		try {
			m.printa();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

class PrintB implements Runnable {
	private Main m;

	public PrintB(Main m) {
		this.m = m;
	}

	@Override
	public void run() {
		try {
			m.printb();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

class PrintC implements Runnable {
	private Main m;

	public PrintC(Main m) {
		this.m = m;
	}

	@Override
	public void run() {
		try {
			m.printc();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```

2,使用Lock+Condition实现
```
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Main {
	public static void main(String[] args) {
		PrintABCUsingCondition printABC = new PrintABCUsingCondition();
		new Thread(() -> printABC.printA()).start();
		new Thread(() -> printABC.printB()).start();
		new Thread(() -> printABC.printC()).start();
	}
}

class PrintABCUsingCondition {
	private Lock lock = new ReentrantLock();
	private Condition conditionA = lock.newCondition();
	private Condition conditionB = lock.newCondition();
	private Condition conditionC = lock.newCondition();
	private int state = 0;

	public void printA() {
		print("A", 0, conditionA, conditionB);
	}

	public void printB() {
		print("B", 1, conditionB, conditionC);
	}

	public void printC() {
		print("C", 2, conditionC, conditionA);
	}

	private void print(String name, int currentState, Condition currentCondition, Condition nextCondition) {
		for (int i = 0; i < 10;) {
			lock.lock();
			try {
				while (state % 3 != currentState) {
					currentCondition.await();
				}
				System.out.println(Thread.currentThread().getName() + " print " + name);
				state++;
				i++;
				nextCondition.signal();
			} catch (InterruptedException e) {
				e.printStackTrace();
			} finally {
				lock.unlock();
			}
		}
	}
}
```

3,使用Semaphore实现
```
import java.util.concurrent.Semaphore;

public class Main {
	public static void main(String[] args) {
		PrintABCUsingSemaphore printABC = new PrintABCUsingSemaphore();
		new Thread(() -> printABC.printA()).start();
		new Thread(() -> printABC.printB()).start();
		new Thread(() -> printABC.printC()).start();
	}
}

class PrintABCUsingSemaphore {
	private Semaphore semaphoreA = new Semaphore(1);
	private Semaphore semaphoreB = new Semaphore(0);
	private Semaphore semaphoreC = new Semaphore(0);

	public void printA() {
		print("A", semaphoreA, semaphoreB);
	}

	public void printB() {
		print("B", semaphoreB, semaphoreC);
	}

	public void printC() {
		print("C", semaphoreC, semaphoreA);
	}

	private void print(String name, Semaphore currentSemaphore, Semaphore nextSemaphore) {
		for (int i = 0; i < 10;) {
			try {
				currentSemaphore.acquire();
				System.out.println(Thread.currentThread().getName() + " print " + name);
				i++;
				nextSemaphore.release();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```
