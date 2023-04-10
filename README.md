# Estrutura_de_Dados_2
Space used to describe the challenges encountered in carrying out the assignments in the third week of the Data Structures 2 course.

3.0 Doubly Linked Lists
For all LinkedList-related questions, unless specified otherwise, assume the LinkedListand Node classes defined below.


# To produce the file linkedlist.py, please remove the comment symbol '#' from the beginning of the line below.
%%file linkedlist.py

class Node:
    """A class representing a node in a doubly linked list."""

    def __init__(self, data):
        """Initialize a new node with the given data."""
        self.data = data
        self.prev = None
        self.next = None

class LinkedList:
    """A class representing a doubly linked list."""

    def __init__(self):
        """Initialize an empty linked list."""
        self.head = None
        self.tail = None
        self.length = 0
        
    def append(self, data):
        """Add a new node with the given data to the end of the linked list."""
        new_node = Node(data)
        if self.length == 0:
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node
        self.length += 1
        
    def __iter__(self):
        """Return an iterator for the linked list."""
        self._iter_node = self.head
        return self 
    
    def __next__(self):
        """Return the next value in the linked list."""
        if self._iter_node is None:
            raise StopIteration
        ret = self._iter_node.data
        self._iter_node = self._iter_node.next
        return ret
    
    def prepend(self, data):
        """Add a new node with the given data to the beginning of the linked list."""
        new_node = Node(data)
        if self.length == 0:
            self.head = self.tail = new_node
        else:
            self.head.prev = new_node
            new_node.next = self.head
            self.head = new_node
        self.length += 1
        
    def __len__(self):
        """Return the length of the linked list."""
        return self.length
    
    def __str__(self):
        """Return a string representation of the linked list."""
        return str([value for value in self])

    def __eq__(self, other):
        """Check if two linked lists are equal.

        Traverse both linked lists and compare the data of each node. 
        If the data of all nodes in both linked lists match, return True. 
        Otherwise, return False.

        Args:
            other (LinkedList): The linked list to compare with self.

        Returns:
            bool: True if the linked lists are equal, False otherwise.
        """
        # Check if the lengths of the linked lists are the same
        if len(self) != len(other):
            print(self,other)
            return False
        
        # Iterate over both linked lists and compare the data of each node
        for node1, node2 in zip(self, other):
            if node1 != node2:
                print(node1.data,node2.data)
                return False
        
        # If we made it this far, the linked lists are equal
        return True
     
Writing linkedlist.py
3.1 Remove Duplicates From Linked List
Difficult: 😃 easy
You're given the head of a doubly Linked List whose nodes are in sorted order with respect to their values. Write a function that returns a modified version of the Linked List that doesn't contain any node with duplicat values. The Linked List should be modified in place (i.e, you shouldn't create a brand new list), and the modified Linked List should still have its nodes sorted with respect to their values.

Each LinkedList node is defined as described in Section 3.0.


%%file test_remove_duplicaiton_from_linked_list_solution.py
import pytest
from linkedlist import*


def removeDuplicatesFromLinkedList(linkedList):
    """Remove duplicates from a linked list and return the modified linked list."""
    node_set = set()
    current_node = linkedList.head
    while current_node is not None:
        if current_node.data in node_set:
            # Remove the node if its data is a duplicate
            next_node = current_node.next
            if current_node.prev is not None:
                current_node.prev.next = current_node.next
            else:
                linkedList.head = current_node.next
            if current_node.next is not None:
                current_node.next.prev = current_node.prev
            else:
                linkedList.tail = current_node.prev
            linkedList.length -= 1
            current_node = next_node
        else:
            # Add the node's data to the set if it is not a duplicate
            node_set.add(current_node.data)
            current_node = current_node.next
    return linkedList



@pytest.fixture(scope="session")
def data():
    
    array = []
    
    # test 1 data
    array.append([1,1,1,3,4,4,4,5,6,6])

    # test 2 data
    array.append([1,1,1,1,1,4,4,5,6,6])

    # test 3 data
    array.append([1,1,1,1,1,1,1])

    # test 4 data
    array.append([1,9,11,15,15,16,17])

    # test 5 data
    array.append([1])

    # test 6 data
    array.append([-5,-1,-1,-1,5,5,5,8,8,9,10,11,11])

    # test 7 data
    array.append([1,2,3,4,5,6,7,8,9,10,11,12,12])
    
    return array

def test_1(data):
    """
    Test evaluation for [1,1,1,3,4,4,4,5,6,6] 
    """
    linkedlist = LinkedList()
    for item in data[0]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1,3,4,5,6]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test


def test_2(data):
    """
    Test evaluation for [1,1,1,1,1,4,4,5,6,6] 
    """
    linkedlist = LinkedList()
    for item in data[1]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1,4,5,6]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test

def test_3(data):
    """
    Test evaluation for [1,1,1,1,1,1,1] 
    """
    linkedlist = LinkedList()
    for item in data[2]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test

def test_4(data):
    """
    Test evaluation for [1,9,11,15,15,16,17] 
    """
    linkedlist = LinkedList()
    for item in data[3]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1,9,11,15,16,17]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test

def test_5(data):
    """
    Test evaluation for [1] 
    """
    linkedlist = LinkedList()
    for item in data[4]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test

def test_6(data):
    """
    Test evaluation for [-5,-1,-1,-1,5,5,5,8,8,9,10,11,11]
    """
    linkedlist = LinkedList()
    for item in data[5]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [-5,-1,5,8,9,10,11]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test

def test_7(data):
    """
    Test evaluation for [1,2,3,4,5,6,7,8,9,10,11,12,12]
    """
    linkedlist = LinkedList()
    for item in data[6]:
      linkedlist.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2,3,4,5,6,7,8,9,10,11,12]:
      linkedlist_test.append(item)

    assert removeDuplicatesFromLinkedList(linkedlist) == linkedlist_test
     
Writing test_remove_duplicaiton_from_linked_list_solution.py

!pytest test_remove_duplicaiton_from_linked_list_solution.py -vv
     
============================= test session starts ==============================
platform linux -- Python 3.9.16, pytest-7.2.2, pluggy-1.0.0 -- /usr/bin/python3
cachedir: .pytest_cache
rootdir: /content
plugins: anyio-3.6.2
collected 7 items                                                              

test_remove_duplicaiton_from_linked_list_solution.py::test_1 PASSED      [ 14%]
test_remove_duplicaiton_from_linked_list_solution.py::test_2 PASSED      [ 28%]
test_remove_duplicaiton_from_linked_list_solution.py::test_3 PASSED      [ 42%]
test_remove_duplicaiton_from_linked_list_solution.py::test_4 PASSED      [ 57%]
test_remove_duplicaiton_from_linked_list_solution.py::test_5 PASSED      [ 71%]
test_remove_duplicaiton_from_linked_list_solution.py::test_6 PASSED      [ 85%]
test_remove_duplicaiton_from_linked_list_solution.py::test_7 PASSED      [100%]

============================== 7 passed in 0.06s ===============================
3.2 Merge Doubly Linked List
Difficult: ⚠️ Medium
Write a function that takes two doubly linked lists that are in sorted order, respectively. The function should merge the lists in place (i.e., it shouldn't create a brand new list) and return the head of the merged list; the merged list should be in sorted order.

Each LinkedList node is defined as described in Section 3.0.

You can assume that the input doubly linked lists will always have at least one node; in other words, the heads will never be None/Null.


%%file test_mergeLinkedLists.py
import pytest
from linkedlist import *

def mergeLinkedLists(linkedList_one, linkedList_two):
    """Junta duas linked lists ordenadas em uma única linked list.

    Args:
        linkedList_one (LinkedList): A primeira linked list.
        linkedList_two (LinkedList): A segunda linked list.

    Returns:
        LinkedList: Uma nova linked list contendo todos os elementos de ambas as listas, em ordem crescente.
    """
    # Cria uma nova linked list vazia para armazenar a lista mesclada
    merged_list = LinkedList()

    # Obter o primeiro nó de ambas as listas
    current_one = linkedList_one.head
    current_two = linkedList_two.head

    # Percorrer ambas as listas enquanto há elementos em ambas
    while current_one is not None and current_two is not None:
        # Se o elemento na primeira lista for menor ou igual, adicioná-lo à lista mesclada e avançar o ponteiro correspondente
        if current_one.data <= current_two.data:
            merged_list.append(current_one.data)
            current_one = current_one.next
        # Caso contrário, adicionar o elemento da segunda lista à lista mesclada e avançar o ponteiro correspondente
        else:
            merged_list.append(current_two.data)
            current_two = current_two.next

    # Se a primeira lista ainda tiver elementos, adicioná-los à lista mesclada
    while current_one is not None:
        merged_list.append(current_one.data)
        current_one = current_one.next

    # Se a segunda lista ainda tiver elementos, adicioná-los à lista mesclada
    while current_two is not None:
        merged_list.append(current_two.data)
        current_two = current_two.next

    # Retornar a lista mesclada
    return merged_list



@pytest.fixture(scope="session")
def data():
    
    array = []
    
    # test 1 data
    array.append([[2,6,7,8],[1,3,4,5,9,10]])

    # test 2 data
    array.append([[1,2,3,4,5],[6,7,8,9,10]])

    # test 3 data
    array.append([[6,7,8,9,10],[1,2,3,4,5]])

    # test 4 data
    array.append([[1,3,5,7,9],[2,4,6,8,10]])

    # test 5 data
    array.append([[0,1,2,3,4,5,7,8,9,10],[6]])

    # test 6 data
    array.append([[6],[0,1,2,3,4,5,7,8,9,10]])

    # test 7 data
    array.append([[1],[2]])

    # test 8 data
    array.append([[2],[1]])

    # test 9 data
    array.append([[1,1,1,3,4,5,5,5,10],[1,1,2,2,5,6,10,10]])
    
    return array

def test_1(data):
    """
    Test evaluation for [[2,6,7,8],[1,3,4,5,9,10]]
    """
    linkedlist_one = LinkedList()
    for item in data[0][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[0][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test


def test_2(data):
    """
    Test evaluation for [[1,2,3,4,5],[6,7,8,9,10]]
    """
    linkedlist_one = LinkedList()
    for item in data[1][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[1][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_3(data):
    """
    Test evaluation for [[6,7,8,9,10],[1,2,3,4,5]]
    """
    linkedlist_one = LinkedList()
    for item in data[2][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[2][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_4(data):
    """
    Test evaluation for [[1,3,5,7,9],[2,4,6,8,10]]
    """
    linkedlist_one = LinkedList()
    for item in data[3][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[3][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_5(data):
    """
    Test evaluation for [[0,1,2,3,4,5,7,8,9,10],[6]]
    """
    linkedlist_one = LinkedList()
    for item in data[4][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[4][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [0,1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_6(data):
    """
    Test evaluation for [[6],[0,1,2,3,4,5,7,8,9,10]]
    """
    linkedlist_one = LinkedList()
    for item in data[5][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[5][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [0,1,2,3,4,5,6,7,8,9,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_7(data):
    """
    Test evaluation for [[1],[2]]
    """
    linkedlist_one = LinkedList()
    for item in data[6][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[6][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_8(data):
    """
    Test evaluation for [[2],[1]]
    """
    linkedlist_one = LinkedList()
    for item in data[7][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[7][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,2]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

def test_9(data):
    """
    Test evaluation for [[1,1,1,3,4,5,5,5,10],[1,1,2,2,5,6,10,10]]
    """
    linkedlist_one = LinkedList()
    for item in data[8][0]:
      linkedlist_one.append(item)

    linkedlist_two = LinkedList()
    for item in data[8][1]:
      linkedlist_two.append(item)

    linkedlist_test = LinkedList()
    for item in [1,1,1,1,1,2,2,3,4,5,5,5,5,6,10,10,10]:
      linkedlist_test.append(item)

    assert mergeLinkedLists(linkedlist_one, linkedlist_two) == linkedlist_test

     
Writing test_mergeLinkedLists.py

!pytest test_mergeLinkedLists.py -vv
     
============================= test session starts ==============================
platform linux -- Python 3.9.16, pytest-7.2.2, pluggy-1.0.0 -- /usr/bin/python3
cachedir: .pytest_cache
rootdir: /content
plugins: anyio-3.6.2
collected 9 items                                                              

test_mergeLinkedLists.py::test_1 PASSED                                  [ 11%]
test_mergeLinkedLists.py::test_2 PASSED                                  [ 22%]
test_mergeLinkedLists.py::test_3 PASSED                                  [ 33%]
test_mergeLinkedLists.py::test_4 PASSED                                  [ 44%]
test_mergeLinkedLists.py::test_5 PASSED                                  [ 55%]
test_mergeLinkedLists.py::test_6 PASSED                                  [ 66%]
test_mergeLinkedLists.py::test_7 PASSED                                  [ 77%]
test_mergeLinkedLists.py::test_8 PASSED                                  [ 88%]
test_mergeLinkedLists.py::test_9 PASSED                                  [100%]

============================== 9 passed in 0.04s ===============================
4.0 Stack

# To produce the file stack.py, please remove the comment symbol '#' from the beginning of the line below.
# Note you need to generate the file linkedlist.py in the Section 3.

%%file stack.py
from linkedlist import *

class Stack(LinkedList):
    """
    This class represents a stack data structure that inherits from a linked list.
    
    Methods:
    push(data) -- adds a new node to the top of the stack with the given data.
    peek() -- returns the data at the top of the stack without removing it.
    pop() -- removes and returns the node at the top of the stack.
    """
    
    def push(self, data):
        """
        Adds a new node to the top of the stack with the given data.
        
        Arguments:
        data -- the data to be stored in the new node.
        """
        self.append(data)

    def peek(self):
        """
        Returns the data at the top of the stack without removing it.
        """
        return self.tail.data

    def pop(self):
        """
        Removes and returns the node at the top of the stack.
        
        Returns:
        The data stored in the node at the top of the stack.
        """
        ret = self.tail.data
        if self.length == 1:
            self.tail = self.head = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        self.length -= 1
        return ret
     
Writing stack.py
4.1 Sunset Views
Difficult: ⚠️ Medium
Given an array of buildings and a direction that all of the buildings face, return an array of the indices of the buildings that can see the sunset.

A building can see the sunset if its strictly taller than all of the buildings that come after it in the direction that it faces.

The input array named buildings contains positive, non-zero integers representing the heights of the buildings. A building at index i thus has a height denoted by buildings[1]. All of the buildings face the same direction, and this direction is either east or west, denoted by the input string named direction, which will always be equal to either EAST or WEST. In relation to the input array, you can interpret these directions as right for east and left for west.

Important note: the indices in the ouput array should be sorted in ascending order.

Sample Input
buildings = [3,5,4,4,3,1,3,2] direction = "EAST"

Sample Output
Below is the visual representation of the sample input
"""_ | |_ _ | | | | _ | | | | | | | |_ | | | | | || | | ||||||||_| """ output = [1,3,6,7]

Sample Input
buildings = [3,5,4,4,3,1,3,2] direction = "WEST"

Sample Output
ouput = [0,1]


%%file test_stack.py
import pytest
from stack import *

def sunsetViews(buildings, direction):
    # Criação de uma pilha vazia para armazenar os índices dos prédios com vista para o pôr do sol
    result = Stack()
    
    # Loop reverso pelos prédios a partir do último prédio
    if direction == "EAST":
        # Se a pilha estiver vazia ou o prédio atual for maior do que o último prédio da pilha
        for i in range(len(buildings)-1, -1, -1):
            # Empilha o índice do prédio atual
            if result.length == 0 or buildings[i] > buildings[result.peek()]:
                result.push(i)
    
    elif direction == "WEST":
        # Loop pelos prédios a partir do primeiro prédio
        for i in range(len(buildings)):
            # Se a pilha estiver vazia ou o prédio atual for maior do que o último prédio da pilha
            if result.length == 0 or buildings[i] > buildings[result.peek()]:
                # Empilha o índice do prédio atual
                result.push(i)
    # Retorna os índices dos prédios com vista para o pôr do sol em ordem crescente
    return sorted(list(result))

@pytest.fixture(scope="session")
def data():
    
    array = []
    
    # test 1 data
    array.append([3, 5, 4, 4, 3, 1, 3, 2])

    # test 2 data
    array.append([3, 5, 4, 4, 3, 1, 3, 2])

    # test 3 data
    array.append([10, 11])

    # test 4 data
    array.append([2,4])

    # test 5 data
    array.append([1])

    # test 6 data
    array.append([1])

    # test 7 data
    array.append([])

    # test 8 data
    array.append([])

    # test 9 data
    array.append([7, 1, 7, 8, 9, 8, 7, 6, 5, 4, 2, 5])

    # test 10 data
    array.append([1, 2, 3, 4, 5, 6])

    # test 11 data
    array.append([1, 2, 3, 4, 5, 6])

    # test 12 data
    array.append([1, 2, 3, 1, 5, 6, 9, 1, 9, 9, 11, 10, 9, 12, 8])

    # test 13 data
    array.append([20, 2, 3, 1, 5, 6, 9, 1, 9, 9, 11, 10, 9, 12, 8])
    
    return array

def test_1(data):
    """
    Test evaluation for [3, 5, 4, 4, 3, 1, 3, 2],EAST
    """
    buildings = data[0]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [1, 3, 6, 7]


def test_2(data):
    """
    Test evaluation for [3, 5, 4, 4, 3, 1, 3, 2],WEST
    """
    buildings = data[1]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == [0,1]

def test_3(data):
    """
    Test evaluation for [10, 11],EAST
    """
    buildings = data[2]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [1]

def test_4(data):
    """
    Test evaluation for [2,4],WEST
    """
    buildings = data[3]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == [0,1]

def test_5(data):
    """
    Test evaluation for [1],EAST
    """
    buildings = data[4]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [0]

def test_6(data):
    """
    Test evaluation for [1],WEST
    """
    buildings = data[5]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == [0]

def test_7(data):
    """
    Test evaluation for [],EAST
    """
    buildings = data[6]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == []

def test_8(data):
    """
    Test evaluation for [],WEST
    """
    buildings = data[7]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == []

def test_9(data):
    """
    Test evaluation for [7, 1, 7, 8, 9, 8, 7, 6, 5, 4, 2, 5],EAST
    """
    buildings = data[8]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [4, 5, 6, 7, 11]

def test_10(data):
    """
    Test evaluation for [1, 2, 3, 4, 5, 6],EAST
    """
    buildings = data[9]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [5]

def test_11(data):
    """
    Test evaluation for [1, 2, 3, 4, 5, 6],WEST
    """
    buildings = data[10]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == [0, 1, 2, 3, 4, 5]

def test_12(data):
    """
    Test evaluation for [1, 2, 3, 1, 5, 6, 9, 1, 9, 9, 11, 10, 9, 12, 8],WEST
    """
    buildings = data[11]
    direction = "WEST"
    assert sunsetViews(buildings, direction) == [0, 1, 2, 4, 5, 6, 10, 13]

def test_13(data):
    """
    Test evaluation for [20, 2, 3, 1, 5, 6, 9, 1, 9, 9, 11, 10, 9, 12, 8],EAST
    """
    buildings = data[12]
    direction = "EAST"
    assert sunsetViews(buildings, direction) == [0, 13, 14]

     
Overwriting test_stack.py

!pytest test_stack.py -vv
     
============================= test session starts ==============================
platform linux -- Python 3.9.16, pytest-7.2.2, pluggy-1.0.0 -- /usr/bin/python3
cachedir: .pytest_cache
rootdir: /content
plugins: anyio-3.6.2
collected 13 items                                                             

test_stack.py::test_1 PASSED                                             [  7%]
test_stack.py::test_2 PASSED                                             [ 15%]
test_stack.py::test_3 PASSED                                             [ 23%]
test_stack.py::test_4 PASSED                                             [ 30%]
test_stack.py::test_5 PASSED                                             [ 38%]
test_stack.py::test_6 PASSED                                             [ 46%]
test_stack.py::test_7 PASSED                                             [ 53%]
test_stack.py::test_8 PASSED                                             [ 61%]
test_stack.py::test_9 PASSED                                             [ 69%]
test_stack.py::test_10 PASSED                                            [ 76%]
test_stack.py::test_11 PASSED                                            [ 84%]
test_stack.py::test_12 PASSED                                            [ 92%]
test_stack.py::test_13 PASSED                                            [100%]

============================== 13 passed in 0.07s ==============================
