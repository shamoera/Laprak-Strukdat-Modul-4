# <h1 align="center">Laporan Praktikum Modul 4 - Singly Linked</h1>
<p align="center">Shafira Shifa Azahra - 109082500125</p>

## Dasar Teori
Linked list atau senarai berantai adalah struktur data yang terdiri dari sejumlah node yang dihubungkan secara linier dengan bantuan pointer, dengan ukuran yang dapat berubah secara dinamis selama program berjalan [1]. Berbeda dengan array yang ukurannya tetap, linked list mengalokasikan memori hanya saat node dibutuhkan, sehingga penambahan dan penghapusan data lebih fleksibel [1].
contoh :
Linked list atau yang disebut juga senarai berantai adalah Salah satu bentuk struktur data yang berisi kumpulan data yang tersusun secara sekuensial, saling bersambungan, dinamis, dan terbatas[1]. Linked list terdiri dari sejumlah node atau simpul yang dihubungkan secara linier dengan bantuan pointer.

### A. Singly Linked List <br/>
Singly linked list adalah linked list yang setiap node-nya hanya memiliki satu pointer (next) yang menunjuk ke node berikutnya, sehingga penelusuran hanya dapat dilakukan satu arah, dari node pertama (First) hingga node terakhir yang menunjuk ke Nil [2].

#### 1. Struktur Node
Setiap node terdiri dari dua bagian, yaitu info untuk menyimpan data dan next untuk menyimpan alamat node berikutnya [2].


# 2. Operasi pada Singly Linked List <br/>
- Insert: menambah node di awal, setelah node tertentu, atau di akhir list [2].
- Delete: menghapus node di awal, di akhir, atau setelah node tertentu. Pointer harus diatur ulang lebih dulu agar sambungan list tidak terputus [2].
- Traversal: menelusuri list dari node pertama sampai Nil, misalnya untuk menampilkan isi atau menghitung jumlah node [2].

## 3. Manajemen Memori
Dalam C++, node dibuat dengan new (alokasi) dan dikembalikan ke sistem dengan delete (dealokasi) agar tidak terjadi kebocoran memori [1].

## Guided 


#### 1. Struktur Node

```C++
//hear guard digunakan untuk mencegah file header yg sama
//di-include lebih dari sekali dalam 1 program
#ifndef LIST_H
#define LIST_H
#define Nil NULL

#include<iostream>
using namespace std;

//deklarasi isi data struct mahasiswa
struct mahasiswa {
    string nama;
    string nim;
    int umur;
};

typedef mahasiswa dataMahasiswa; //memberikan nama alias dataMahasiswa untuk struct mahasiswa

typedef struct node *address; //mendefinisikan alias address sebagai pointer ke struct node

struct node { //node untuk isi dari linked listnya, isis setiap node adalah data & pointer next
    dataMahasiswa isidata;
    address next;
};

struct linkedList { //ini linked listnya
    address first;
};

//semua function & prosedur yang akan dipakai
bool isEmpty(linkedList list);
void createList(linkedList &list);
address alokasi(string nama, string nim, int umur);
void dealokasi(address &node);
void printList(linkedList list);
void insertFirst(linkedList &list, address nodeBaru);
void insertAfter(linkedList &list, address nodeBaru, address Prev);
void insertLast(linkedList &list, address nodeBaru);

void delFirst(linkedList &list);
void delLast(linkedList &list);
void delAfter(linkedList &list, address nodeHapus, address nodePrev);
int nbList(linkedList list);
void deleteList(linkedList &list);

#endif
```
Berisi header guard, definisi Nil, struct mahasiswa, node, dan linkedList, serta deklarasi seluruh fungsi dan prosedur. linkedList hanya menyimpan pointer first yang menunjuk ke node pertama.

### 2. list.cpp

```C++
#include "list.h"
#include <iostream>
using namespace std;

//I.S = Initial State / kondisi awal
//F.S = Final State / kondisi akhir

//fungsi untuk cek apakah list kosong atau tidak
bool isEmpty(linkedList List) {
    if(List.first == Nil){
        return true;
    } else {
        return false;
    }
}

//pembuatan linked list kosong
void createList(linkedList &List) {
    /* I.S. sembarang
       F.S. terbentuk list kosong */
    List.first = Nil;
}

//pembuatan node baru dengan menerapkan manajemen memori
address alokasi(string nama, string nim, int umur) {
    /* I.S. sembarang
       F.S. mengembalikan alamat node baru dengan isidata = sesuai parameter dan next = Nil */
    address nodeBaru = new node;
    nodeBaru->isidata.nama = nama;
    nodeBaru->isidata.nim = nim;
    nodeBaru->isidata.umur = umur;
    nodeBaru->next = Nil;
    return nodeBaru;
}

//penghapusan node dengan menerapkan manajemen memori
void dealokasi(address &node) {
    /* I.S. P terdefinisi
       F.S. memori yang digunakan node dikembalikan ke sistem */
    node->next = Nil;
    delete node;
}

//prosedur-prosedur untuk insert / menambahkan node baru kedalam list
void insertFirst(linkedList &List, address nodeBaru) {
    /* I.S. sembarang, P sudah dialokasikan
       F.S. menempatkan elemen list (node) pada awal list */
    nodeBaru->next = List.first;
    List.first = nodeBaru;
}

void insertAfter(linkedList &List, address nodeBaru, address Prev) {
    /* I.S. sembarang, nodeBaru dan Prev alamat salah satu elemen list (node)
       F.S. menempatkan elemen (node) sesudah elemen node Prev */
    if (Prev != Nil) {
        nodeBaru->next = Prev->next;
        Prev->next = nodeBaru;
    } else {
        cout << "Node sebelumnya tidak valid!" << endl;
    }
}

void insertLast(linkedList &List, address nodeBaru) {
    /* I.S. sembarang, nodeBaru sudah dialokasikan
       F.S. menempatkan elemen nodeBaru pada akhir list */
    if (isEmpty(List)) {
        List.first = nodeBaru;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        nodeBantu->next = nodeBaru;
    }
}

//prosedur-prosedur untuk delete / menghapus node yang ada didalam list
void delFirst(linkedList &List){
    /* I.S. list tidak kosong
    F.S. node pertama di list terhapus*/
    address nodeHapus;
    if (isEmpty(List) == false) {
        nodeHapus = List.first;
        List.first = List.first->next;
        nodeHapus->next = Nil;
        dealokasi(nodeHapus);
    } else {
        cout << "List kosong!" << endl;
    }
}

void delLast(linkedList &List){
    /* I.S. list tidak kosong
    F.S. node terakhir di list terhapus */
    address nodeHapus, nodePrev;
    if(isEmpty(List) == false){
        nodeHapus = List.first;
        if(nodeHapus->next == Nil){
            List.first->next = Nil;
            dealokasi(nodeHapus);
        } else {
            while(nodeHapus->next != Nil){
                nodePrev = nodeHapus;
                nodeHapus = nodeHapus->next;
            }
            nodePrev->next = Nil;
            dealokasi(nodeHapus);
        }
    } else {
        cout << "list kosong" << endl;
    }
}

void delAfter(linkedList &List, address nodeHapus, address nodePrev){
    /* I.S. list tidak kosng, Prev alamat salah satu elemen list
    F.S. nodeBantu adalah alamat dari Prev->next, menghapus Prev->next dari list */
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else { //jika list tidak kosong
        if (nodePrev != Nil && nodePrev->next != Nil) {
            nodeHapus = nodePrev->next;
            nodePrev->next = nodeHapus->next;
            nodeHapus->next = Nil;
            dealokasi(nodeHapus);
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

//prosedur untuk menampilkan isi list
void printList(linkedList List) {
    /* I.S. list mungkin kosong
       F.S. jika list tidak kosong menampilkan semua info yang ada pada list */
    if (isEmpty(List)) {
        cout << "List kosong." << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu != Nil) {
            cout << "Nama : " << nodeBantu->isidata.nama << ", NIM : " << nodeBantu->isidata.nim << ", Usia : " << nodeBantu->isidata.umur << endl;
            nodeBantu = nodeBantu->next;
        }
    }
}

//function untuk menampilkan jumlah node didalam list
int nbList(linkedList List) {
    /* I.S. list sudah ada
       F.S. menampilkan jumlah node didalam list*/
    int count = 0;
    address nodeBantu = List.first;
    while (nodeBantu != Nil) {
        count++;
        nodeBantu = nodeBantu->next;
    }
    return count;
}

//prosedur untuk menghapus list (menghapus semua node didalam list)
void deleteList(linkedList &List){
    /* I.S. list sudah ada
       F.S. menghapus semua node didalam list*/
    address nodeBantu, nodeHapus;
    nodeBantu = List.first;
    while(nodeBantu != Nil){
        nodeHapus = nodeBantu;
        nodeBantu = nodeBantu->next;
        dealokasi(nodeHapus);
    }
    List.first = Nil;
    cout << "List sudah terhapus!" << endl;
}
```
Berisi implementasi fungsi dan prosedur:

- isEmpty, createList: mengecek dan membuat list kosong.
- alokasi, dealokasi: membuat node baru dan mengembalikan memorinya ke sistem.
- insertFirst, insertAfter, insertLast: menambah node di awal, setelah node tertentu, dan di akhir list.
- delFirst, delLast, delAfter: menghapus node pertama, terakhir, dan setelah node tertentu.
- printList, nbList: menampilkan isi list dan menghitung jumlah node.
- deleteList: menghapus seluruh node pada list.

### 3. main.cpp

```C++
#include "list.h"

#include<iostream>
using namespace std;

int main(){
    linkedList List;
    address nodeA, nodeB, nodeC, nodeD, nodeE = Nil;
    createList(List);

    dataMahasiswa mhs;

    nodeA = alokasi("Dhimas", "2311102151", 20);
    nodeB = alokasi("Arvin", "2211110014", 21);
    nodeC = alokasi("Rizal", "2311110029", 20);
    nodeD = alokasi("Satrio", "2211102173", 21);
    nodeE = alokasi("Joshua", "2311102133", 21);

    insertFirst(List, nodeA);
    insertLast(List, nodeB);
    insertAfter(List, nodeC, nodeA);
    insertAfter(List, nodeD, nodeC);
    insertLast(List, nodeE);

    cout << "--- ISI LIST SETELAH DILAKUKAN INSERT ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    delFirst(List);
    delLast(List);
    delAfter(List, nodeD, nodeC);

    cout << "--- ISI LIST SETELAH DILAKUKAN DELETE ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    deleteList(List);
    cout << "--- ISI LIST SETELAH DILAKUKAN HAPUS LIST ---" << endl;
    printList(List);
    cout << "jumlah node : " << nbList(List) << endl;
    cout << endl;

    return 0;
}
```
Program utama membuat list kosong, mengalokasikan lima node (Dhimas, Arvin, Rizal, Satrio, Joshua), lalu melakukan operasi insert sehingga urutan list menjadi Dhimas → Rizal → Satrio → Arvin → Joshua (5 node). Selanjutnya dilakukan operasi delete (delFirst, delLast, delAfter) sehingga tersisa Rizal → Arvin (2 node). Terakhir, deleteList menghapus seluruh isi list sehingga jumlah node menjadi 0.

## Unguided 

### 1. Buatlah ADT Singly Linked List pada file Singlylist.h (infotype: int, ElmList berisi info dan next, List berisi First) dengan prosedur createList, alokasi, dealokasi, printInfo, dan insertFirst. Implementasikan di Singlylist.cpp, lalu buat main.cpp yang mengalokasikan dan memasukkan node 2, 0, 8, 12, dan 9 menggunakan insertFirst, kemudian menampilkan isi list.

# Singlylist.h
```C++
#ifndef SINGLYLIST_H
#define SINGLYLIST_H
#define Nil NULL

#include <iostream>
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);

#endif
```

# Singlylist.cpp
```C++
#include "Singlylist.h"

// membuat list kosong
void createList(List &L) {
    L.First = Nil;
}

// membuat node baru dengan info = x
address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

// mengembalikan memori node ke sistem
void dealokasi(address &P) {
    P->next = Nil;
    delete P;
    P = Nil;
}

// menampilkan isi list dari node pertama sampai terakhir
void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

// menambah node P di awal list
void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}
```

# main.cpp
```C++
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    createList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    printInfo(L);
    return 0;
}
```
### Output Unguided 1 :

##### Output 
![Screenshot Output Unguided 1_1](https://github.com/shamoera/Laprak-Strukdat-Modul-4/blob/main/output%20soal%201.png)


Program ini membuat singly linked list yang menyimpan bilangan bulat. Setiap node (ElmList) berisi info (data) dan next (pointer ke node berikutnya). Struktur List hanya menyimpan First, yaitu pointer ke node pertama.

Prosedur yang digunakan:

- createList: membuat list kosong (First = Nil).
- alokasi: membuat node baru dengan new dan mengisi info.
- dealokasi: mengembalikan memori node ke sistem dengan delete.
- insertFirst: menambahkan node di awal list.
- printInfo: menampilkan isi list dari node pertama sampai terakhir.

Pada main, lima node dengan nilai 2, 0, 8, 12, dan 9 dimasukkan menggunakan insertFirst. Karena setiap node baru selalu ditempatkan di depan, urutan list menjadi terbalik dari urutan pemasukan, yaitu 9 → 12 → 8 → 0 → 2. Output program adalah 9 12 8 0 2. 

### 2. Dari soal 1, hapus node 9 dengan deleteFirst(), node 2 dengan deleteLast(), dan node 8 dengan deleteAfter(). Tampilkan jumlah node dengan nbList(), lalu hapus seluruh node dengan deleteList().

# singlylist.h

```C++
#ifndef SINGLYLIST_H
#define SINGLYLIST_H
#define Nil NULL

#include <iostream>
using namespace std;

typedef int infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
};

struct List {
    address First;
};

void createList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);

void deleteFirst(List &L);
void deleteLast(List &L);
void deleteAfter(List &L, address Prec);
int nbList(List L);
void deleteList(List &L);

#endif
```

# singlylist.cpp

```C++
#include "Singlylist.h"

// membuat list kosong
void createList(List &L) {
    L.First = Nil;
}

// membuat node baru dengan info = x
address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

// mengembalikan memori node ke sistem
void dealokasi(address &P) {
    P->next = Nil;
    delete P;
    P = Nil;
}

// menampilkan isi list dari node pertama sampai terakhir
void printInfo(List L) {
    address P = L.First;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

// menambah node P di awal list
void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}

// menghapus node pertama
void deleteFirst(List &L) {
    if (L.First != Nil) {
        address P = L.First;
        L.First = P->next;
        dealokasi(P);
    }
}

// menghapus node terakhir
void deleteLast(List &L) {
    if (L.First != Nil) {
        if (L.First->next == Nil) {
            // hanya ada 1 node
            address P = L.First;
            L.First = Nil;
            dealokasi(P);
        } else {
            address Prev = L.First;
            while (Prev->next->next != Nil) {
                Prev = Prev->next;
            }
            address P = Prev->next;
            Prev->next = Nil;
            dealokasi(P);
        }
    }
}

// menghapus node setelah node Prec
void deleteAfter(List &L, address Prec) {
    if (Prec != Nil && Prec->next != Nil) {
        address P = Prec->next;
        Prec->next = P->next;
        dealokasi(P);
    }
}

// menghitung jumlah node
int nbList(List L) {
    int count = 0;
    address P = L.First;
    while (P != Nil) {
        count++;
        P = P->next;
    }
    return count;
}

// menghapus semua node
void deleteList(List &L) {
    while (L.First != Nil) {
        deleteFirst(L);
    }
    cout << "- List Berhasil Terhapus -" << endl;
}
```

# main.cpp

```C++
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5 = Nil;
    createList(L);

    P1 = alokasi(2);  insertFirst(L, P1);
    P2 = alokasi(0);  insertFirst(L, P2);
    P3 = alokasi(8);  insertFirst(L, P3);
    P4 = alokasi(12); insertFirst(L, P4);
    P5 = alokasi(9);  insertFirst(L, P5);

    deleteFirst(L);        // hapus node 9
    deleteLast(L);         // hapus node 2
    deleteAfter(L, P4);    // hapus node 8 (node setelah 12)

    printInfo(L);
    cout << "Jumlah node : " << nbList(L) << endl;
    cout << endl;

    deleteList(L);
    cout << "Jumlah node : " << nbList(L) << endl;

    return 0;
}
```
### Output Unguided 2 :

##### Output 
![Screenshot Output Unguided 2_1](https://github.com/shamoera/Laprak-Strukdat-Modul-4/blob/main/output%20soal%202.png)

Program ini melanjutkan soal 1 dengan menambahkan operasi penghapusan node:

- deleteFirst: menghapus node pertama.
- deleteLast: menghapus node terakhir.
- deleteAfter: menghapus node yang berada setelah node tertentu.
- nbList: menghitung jumlah node dalam list.
- deleteList: menghapus seluruh node dalam list.

Setelah penghapusan, list berisi 12 0 dengan jumlah node 2. Selanjutnya deleteList menghapus semua node sehingga jumlah node menjadi 0.

## Kesimpulan
Dari praktikum ini dapat disimpulkan bahwa singly linked list menyimpan data secara dinamis menggunakan pointer, di mana setiap node menunjuk ke node berikutnya. Operasi insertFirst menempatkan node baru di awal list, sehingga urutan list menjadi terbalik dari urutan pemasukan. Operasi penghapusan (deleteFirst, deleteLast, deleteAfter, dan deleteList) harus mengatur ulang pointer sebelum node dihapus agar sambungan list tidak terputus. Dengan new dan delete, memori digunakan sesuai kebutuhan sehingga lebih fleksibel dibandingkan array yang ukurannya tetap.

## Referensi
[1] Tim Penyusun Modul Praktikum Struktur Data, "Modul 4: Singly Linked List (Bagian Pertama)," Modul Praktikum Struktur Data, Fakultas Informatika, Telkom University, [tahun]. 
<br>
