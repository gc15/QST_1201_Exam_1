package Test;

public  class LinkList {
	public int value; // 存放节点的值
    public LinkList next = null; // 存放下一个节点
    LinkList(int value){
        this.value = value;
    }
    LinkList(int value, LinkList next){
        this.value = value;
        this.next = next;
    }
    public void setValue(int value){
        this.value = value;
    }

// 实现以下函数，打印链表head
public void printLinkList(LinkList head){
		int a = head.value;
		while (head.next != null) {
			System.out.print(a + "->");
			head = head.next;
			a = head.value;
	
		}
		System.out.print(a);
		  
				

}
}
