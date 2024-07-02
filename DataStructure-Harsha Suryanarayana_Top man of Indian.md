# DataStructure-Harsha Suryanarayana_Top man of Indian

## 1.LinkedList

### 1.Insert At Head

```c
#include <stdio.h>
#include <stdlib.h>

struct Node
{
	int data;
	struct Node* next;
};

struct Node* head;

void Insert(int x);
void Print();

int main(void)
{
	head = NULL;
	printf("How many numbers?\n");
	int n,i,x;
	scanf("%d",&n);
	for(i = 0;i < n;i++)
	{
		printf("Enter the number\n");
		scanf("%d",&x);
		Insert(x);
		Print();
	}
}

void Insert(int x)
{
	struct Node* temp = (struct Node*)malloc(sizeof(struct Node));
	(*temp).data = x;
	temp->next = head;
	head = temp;
}

void Print()
{
	struct Node* temp = head;
	printf("List is: ");
	while(temp != NULL)
	{
		printf(" %d",temp->data);
		temp = temp->next;
	}
	printf("\n");
}
```

```c++
#include <stdio.h>
#include <stdlib.h>

struct Node
{
	int data;
	Node* next;
};

Node* Insert(Node*,int);
void Print(Node*);

int main(void)
{
	Node* head = NULL;
	printf("How many numbers?\n");
	int n,i,x;
	scanf("%d",&n);
	for(i = 0;i < n;i++)
	{
		printf("Enter the number\n");
		scanf("%d",&x);
		head = Insert(head,x);
		Print(head);
	}
}

Node* Insert(Node* head, int x)
{
	Node* temp = (Node*)malloc(sizeof(Node));
	(*temp).data = x;
	temp->next = head;
	return temp;
}

void Print(Node* head)
{
	printf("List is: ");
	while(head != NULL)
	{
		printf(" %d",head->data);
		head = head->next;
	}
	printf("\n");
}
```

### 2.Insert At n

```c++
#include <stdio.h>
#include <stdlib.h>

struct Node
{
	int data;
	Node* next;
};

Node* head;
void Insert(int data,int n);
void Print();

int main(void)
{
	head = NULL;
	//insert At n
	Insert(2,1);
	Insert(3,2);
	Insert(4,1);
	Insert(5,2);
	Print();
}

void Insert(int data,int n)
{
	Node* temp1 = new Node();
	temp1->data = data;
	temp1->next = NULL;
	if(n == 1)
	{
		temp1->next = head;
		head = temp1;
		return;
	}
	Node* temp2 = head;
	for(int i = 0;i < n - 2;i++)
	{
		temp2 = temp2->next;
	}
	temp1->next = temp2->next;
	temp2->next = temp1;
}

void Print()
{
	Node* temp = head;
	while(temp != NULL)
	{
		printf("%d ",temp->data);
		temp = temp->next;
	}
	printf("\n");
}
```

### 3.Delete At n

```c++
#include <stdio.h>
#include <stdlib.h>

struct Node{
	int data;
	struct Node* next;
};

Node* head;
void Insert(int data);
void Print();
void Delete(int n);

int main()
{
	head = NULL;
	Insert(2);
	Insert(4);
	Insert(6);
	Insert(5);//2 4 6 5
	Print();
	int n;
	printf("Enter a position\n");
	scanf("%d",&n);
	Delete(n);
	Print();
}

void Insert(int data){
	Node* temp = new Node();
	temp->data = data;
	temp->next = NULL;
	if(head == NULL){
		head = temp;
		return;
	}
	Node* temp1 = head;
	while(temp1->next != NULL)
	{
		temp1 = temp1->next;
	}
	temp1->next = temp;
			
}

void Print(){
	Node* temp = head;
	while(temp != NULL){
		printf("%d ",temp->data);
		temp = temp->next;
	}
	printf("\n");
}

void Delete(int n){
	Node* temp1 = head;
	if(n == 1)
	{
		head = temp1->next;
		delete temp1;
		return;
	}
	int i;
	for(i = 0;i < n-2;i++)
	{
		temp1 = temp1->next;
	}
	Node* temp2 = temp1->next;
	temp1->next = temp2->next;
	delete temp2;
}


```

### 4.Reverse LinkedList

```c++
#include <stdio.h>
#include <stdlib.h>

struct Node
{
	int data;
	struct Node* next;
};

Node* head;
void Insert(int);
void Print();
void Reverse();

int main()
{
	Insert(2);
	Insert(4);
	Insert(6);
	Insert(5);//2 4 6 5
	Print();
	Reverse();
	Print();
}

void Insert(int data) {
	Node* temp = new Node();
	temp->data = data;
	temp->next = NULL;
	if (head == NULL) {
		head = temp;
		return;
	}
	Node* temp1 = head;
	while (temp1->next != NULL)
	{
		temp1 = temp1->next;
	}
	temp1->next = temp;
}

void Print() {
	Node* temp = head;
	while (temp != NULL) {
		printf("%d ", temp->data);
		temp = temp->next;
	}
	printf("\n");
}

void Reverse() 
{
	Node* current,*prev,*next; 
	current = head;
	prev = NULL;
	while (current != NULL)
	{
		next = current->next;
		current->next = prev;
		prev = current;
		current = next;
	}
	head = prev;
}
```

