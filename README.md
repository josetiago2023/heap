## Tipos de Árvore
Antes de vermos o que é um Heap precisamos conhecer primeiro alguns tipos de árvores, úteis no Heap

### Árvore Binaria
Um nó em uma árvore binária tem no máximo dois filhos: um filho à esquerda e um filho à direita. Essa definição nos permite escrever algoritmos mais eficazes para inserir, pesquisar e remover nós na árvore. As árvores binárias são muito usadas em ciência da computação

* Quando um nó não tem nenhum filho é chamado de folha.
* O primeiro nó é chamado de Raiz

### Árvore Binaria cheia
É uma árvore onde seus nós podem ter 0 ou dois nós filhos (ou folhas), se uma árvore tem um nó que tem um filho, essa não é cheia. Como nessa imagem abaixo ela tanto é cheia quanto completa.

![Arvore Cheia](https://github.com/user-attachments/assets/3f9cc83c-464b-4ebe-bfb8-4dc2a079330a)


### Árvore Binaria completa
É uma árvore cujo preenchimento se dâ da esquerda para a direita. Em uma árvore binária completa todos os níveis, exceto possivelmente o último, está completamente cheia, e todos os nós no último nível são, tanto à esquerda quanto possível.

![image](https://github.com/user-attachments/assets/ecfbca42-7440-4862-9b19-24d27d782d61)

## HEAP

É uma estrutura em forma de árvore Binária e conhecida como Binary Heap ou Heap Binário, essa estrutura é muito útil na a construção de filas com prioridades (cada elemento possui uma prioridade associada), para armazenar conjunto de elementos de forma organizada na árvore (pois uma árvore organizada se dá da esquerda para a direita) e não organizada num array (pois os valores não estarão organizados do maior para o menor).

Assim como a árvore binária a Binary Heap é uma estrutura de árvore binária, onde cada nó possui no máximo dois filhos. Essa estrutura facilita a implementação e manipulação dos elementos, permitindo a utilização de algoritmos eficientes para a inserção, remoção e busca de elementos.

Para poder executar uma estrutura do Heap, seja ela Heap Máximo ou Mínimo é necessário que a estrutura da árvore seja Binária e Completa, pois assim saberemos que o preencimento da árvore será da esquerda para a direita e contém 0, ou 2 valores.

Dentro de Heap existem dois tipos com estruturas diferentes, o Heap Máximo e Heap Mínimo

### Tipos de Heap
#### Heap Máximo
No Heap Máximo é onde a estrutura de Árvore Binária contará com os nós maiores valores sempre em cima.

```js
class MaxHeap {
    constructor() {
      this.heap = []
    }
  
    getLeftIndex(index) {
        return 2 * index + 1 // Obter o índice do filho esquerdo
    }
  
    getRightIndex(index) {
      return 2 * index + 2 // Obter o índice do filho direito
    }
  
    
    getParentIndex(index) {
      if (index === 0) {
        return undefined
      }
      return Math.floor((index - 1) / 2) // Obter o índice do pai
    }
  
    // Função para comparar os valores no heap (positivo sobe)
    compareFn(a, b) {
      return b - a 
    }
  
    // Após inserir o elemento precisamos comparar o elemento, se for maior sobe (siftUp)
    siftUp(index) {
      let parent = this.getParentIndex(index)
      while (
        index > 0 &&
        this.compareFn(this.heap[parent], this.heap[index]) < 0 
      ) {
        this.swap(this.heap, parent, index)
        index = parent
        parent = this.getParentIndex(index)
      }
    }
  
    // Descer um elemento na árvore (apos remover)
    siftDown(index) {
      let element = index
      const left = this.getLeftIndex(index)
      const right = this.getRightIndex(index)
      const size = this.heap.length
  
      // Comparar com o filho esquerdo
      if (
        left < size &&
        this.compareFn(this.heap[element], this.heap[left]) < 0 
      ) {
        element = left
      }
  
      // Comparar com o filho direito
      if (
        right < size &&
        this.compareFn(this.heap[element], this.heap[right]) < 0
      ) {
        element = right
      }
  
      // Trocar
      if (index !== element) {
        this.swap(this.heap, index, element)
        this.siftDown(element)
      }
    }
  
    insert(value) {
      if (value != null) {
        this.heap.push(value)
        this.siftUp(this.heap.length - 1)
        return true
      }
      return false
    }
  
    // Remover o valor máximo (raiz)
    extract() {
      if (this.isEmpty()) {
        return undefined
      }
      if (this.size() === 1) {
        return this.heap.shift()
      }
  
      const removedValue = this.heap.shift() // Remove a raiz
      this.siftDown(0)
      return removedValue
    }
  
    // Mostrar o valor máximo
    findMaximum() {
      return this.isEmpty() ? undefined : this.heap[0]
    }
  
    size() {
      return this.heap.length
    }
  
    isEmpty() {
      return this.size() === 0
    }
  
    // Função para trocar dois elementos no array
    swap(array, a, b) {
      const temp = array[a]
      array[a] = array[b]
      array[b] = temp
    }
}

export {MaxHeap}
```

#### Heap Mínimo
No Heap Mínimo é onde os nós menores valores sempre estrão em cima.
```js
class MaxHeap {
    constructor() {
      this.heap = []
    }
  
    getLeftIndex(index) {
        return 2 * index + 1 // Obter o índice do filho esquerdo
    }
  
    getRightIndex(index) {
      return 2 * index + 2 // Obter o índice do filho direito
    }
  
    
    getParentIndex(index) {
      if (index === 0) {
        return undefined
      }
      return Math.floor((index - 1) / 2) // Obter o índice do pai
    }
  
    compareFn(a, b) {
      return a - b
    }
  
    insert(value) {
      if (value == null) return false
  
      this.heap.push(value) // Para inserir o valor novo sempre será adicionado no final da arvore
      this.siftUp(this.heap.length - 1)
      return true
    }
  
    siftUp(index) {
        let parent = this.getParentIndex(index)
        
        while (index > 0 && this.compareFn(this.heap[parent], this.heap[index]) > 0) {
        // Se o index Menor, sobe valor
        this.swap(this.heap, parent, index)
        index = parent // Atualiza o índice
        parent = this.getParentIndex(index) 
      }
    }
  
    // Trocar posicoes
    swap(array, a, b) {
      [array[a], array[b]] = [array[b], array[a]] 
    }
  
    // Remover o valor mínimo
    extract() {
      if (this.isEmpty()) return undefined 
  
      if (this.size() === 1) {
        return this.heap.shift() 
      }
  
      const removedValue = this.heap.shift()
      this.siftDown(0)
      return removedValue
    }
  
    isEmpty() {
      return this.heap.length === 0
    }
  
    size() {
      return this.heap.length
    }
  
    findMinimum() {
      return this.isEmpty() ? undefined : this.heap[0] // Retorna a raiz ou undefined se vazio
    }
  
    // Se o valor for maior desce
    siftDown(index) {
      let element = index
      const left = this.getLeftIndex(index)
      const right = this.getRightIndex(index)
      const size = this.size()
  
      if (left < size && this.compareFn(this.heap[element], this.heap[left]) > 0) {
        element = left
      }
  
      
      if (right < size && this.compareFn(this.heap[element], this.heap[right]) > 0) {
        element = right 
      }
  
      
      if (index !== element) {
        this.swap(this.heap, index, element) 
        this.siftDown(element) 
      }
    }
  }
  

export {MinHeap}
```
"Tanto à esquerda quanto possível". Caso contrário, os nós vão se sobrepôr."
