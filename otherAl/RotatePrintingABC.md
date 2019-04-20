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
