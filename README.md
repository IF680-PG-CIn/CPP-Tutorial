# CPP-Tutorial

Em C++ lidamos diretamente com conceitos de baixo nível, como alocação de memória e ponteiros. Isso nos permite maior flexibilidade, porém também nos exige mais cuidado.

Um ponteiro nada mais é que uma valor referenciando uma posição na memória.

```c++
#include <iostream>

using namespace std;

void swap(int* a, int* b) {
  int aux = *a;
  *a = *b;
  *b = aux;
}

int main() {
  int a = 1;
  int b = 2;
  swap(&a, &b);
  
  cout << a << " " << b << endl;
  
  return 0;
}
```

Temos também o conceito de referência, que em poucas palavras é um ponteiro que sempre é inicializado na declaração, nunca podendo ser nulo. 

```c++
...
Swap(int& a, int& b) {
  int aux = a;
  a = b;
  b = a;
}

...
  Swap(a, b);
...
```

Para manipular memória em C++ usamos os operadores **new** e **delete**, diferente de C, onde usamos **malloc** e **free**.
Ao usar **new**, é alocado espaço na heap e chamado o construtor do objeto em questão.

```c++
int *a = new int(2);
delete a;

int *b = new int[5];
delete[] b;
```

Em C++ podemos nos beneficiar do uso da STL (Standard Template Library), trazendo estruturas de dados e algorítmos de propósito geral.

```c++
#include <vector>

...

vector<int> v;
v.push_back(1);
v.push_back(2);
for (auto value : v)
  cout << v << endl;

...
```

Em C++ temos fácil acesso à conceitos de OOP como classes, herança, polimorfismo, etc.

Ao criar uma classe em C++, comumente queremos ter um Header(.h), onde declaramos uma interface, e um Source(.cpp) onde implementamos as funções.
Sempre que usamos a diretiva **#include \<file.h>** ou **include \"file.h"** o linker incluirá uma cópia de **file.h** no executável. Para impedir que um programa tenha mais de uma cópia de um mesmo arquivo, encapsulamos o código em uma diretiva **#ifndef**

Marcamos uma função como **virtual** para informar ao compilador que ela pode ser sobrescrita (**virtual type function() = 0** significa que a função é puramente virtual, e deve ser implementada por classes filhas), e com **override** para informar que ela sobrescreve alguma função de uma classe mãe.

> Shape.h
> ```c++
> #ifndef _SHAPE_H
> #define _SHAPE_H
> 
> class Shape {
> public:
>   virtual double GetVolume() = 0;
> };
> 
> class Sphere : public Shape {
> private:
> double radius;
>
> public:
>   Sphere(double radius = 1.0);
>   double GetVolume() override;
> }
> #endif
> ```
>
> Shape.cpp
> ```c++
> #include "Shape.h"
> #include <cmath>
> 
> Sphere::Sphere(double radius) : radius{radius} {}
> 
> double Sphere::GetVolume() {
>   return (4.0 / 3.0) * M_PI * pow(this->radius, 3);
> }
> 
> main.cpp
> ...
> Shape* shape = new Sphere(1.0);
> shape->GetVolume();
  
Em C++ podemos fazer operator overloading, criando novos significados para operadores como +, -, *, etc.

Uma função marcada como **friend** tem acesso à todos os atributos **private** ou **protected** de uma determinada classe.

```c++
class Vec3 {
public:
  double x, y, z;
  
  Vec3(double x = 0, double y = 0, double z = 0) : x{x}, y{y}, z{z} {}
  
  friend Vec3 operator +(const Vec3& left, const Vec3& right) {
    return Vec3(left.x + right.x, left.y + right.y, left.z + right.z);
  }
  
  friend Vec3 operator *(const Vec3& left, const double& right) {
    return Vec3(left.x * right, left.y * right, left.z * right);
  }
  
  friend Vec3 operator *(const double& left, const Vec3& right) {
    return right * left;
  }
}
