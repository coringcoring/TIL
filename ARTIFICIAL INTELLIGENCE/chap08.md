# CHAP 09

## 유전자 알고리즘
* 생물체의 염색체가 유전되는 과정에서 영감을 얻은 `최적화 알고리즘`
* 적자생존 원칙에 기반을 두고 `교차`,`돌연변이`,`도태` 등의 과정을 통하여 우성 유전자만이 살아남는 자연계의 현상을 알고리즘으로 만든 것 
* 여러 세대를 거치는 동안 `적합도(Fitness)`가 높은 부모가 선택되면, 적합도가 더 높은 자식을 낳을 가능성이 높아짐. 
    ex>세대를 반복하면서 결국 목이 긴 기린들의 개체 집단으로 채워질 수 있는 것
* 염색체: 유전자 알고리즘에서 가장 중요한 것. 염색체는 0과 1로 이루어진 문자열로 가정 
* 유전자: 염색체에 들어있는 0과 1 -> 유전자가 여러개 모여서 하나의 염색체를 구성 
    * 인코딩: 문제공간을 염색체들의 형태로 변경
    * 디코딩: 염색체들을 문제 공간으로 역변환하는 것 
* 평가함수: 현재의 염색체가 얼마나 문제를 잘 해결하고 있는지를 나타내는 적합도(fitness)를 반환 
    * 유전자 알고리즘은 지식을 재생산할 때 평가함수를 사용
    * 적합도 값이 우수한 염색체들이 *부모로 선정될 확률이 높아짐* (선정된다는 것이 아니라 확률이 높은거임. 반드시 선정되지 않을 수도 있음)
* 교차(crossover): 재생산이 일어나면 부모의 염색체들의 일부가 교환됨 
* 돌연변이(mutation): 염색체에서 랜덤한 위치의 유전자의 값을 바꿈  
1. 랜덤으로 초기 해답들을 만듦 (최적이 아닐 수도 있음)
2. 가장 좋은 해답을 **`선택`** (fitness function 사용)
3. 유전자 **`교차`**를 통해 다음 세대 생성 
4. 몇 개의 해답을 **`돌연변이`** 시킴 -> 다시 **2**번으로 

### 선택(Selection) 연산자
* 재생산을 위해 우수한 성능을 보이는 부모 염색체 2개를 선택함 
    * `룰렛 휠 선택(rootlet wheel selection)` 알고리즘 많이 사용함
    * 부모 염색체 선택은 [0,100] 사이의 난수를 생성해서 난수 위치의 염색체 선택 
### 교차(Crossover) 연산자
* 염색체 간의 교배를 나타냄
* 선택 연산자로 두 개의 염색체를 선택하고 교차 위치를 임의로 선택 
* 교차 지점을 중심으로 유전자가 서로 교환되어 완전히 새로운 자손을 생성 
### 돌연변이(mutation) 연산자
* 자손에 유전자를 무작위로 삽입하는 연산
* 지역 최소값(지역 최대값)을 피하고, 개체군이 다양성을 유지하기 위함

### 유전자 알고리즘 pseudo 코드 
```
GeneticAlgorithm(population,FitnessFunc){
    repeat
        new_population<-[]
        for i=1 to size(population) do
            father <-select(population,FitnessFunc)
            mother<-select(population,FitnessFunc)
            child<-crossover(father,mother)
            if (난수<변이_확률) then child<-mutate(child)
            new_population<-new_population+child 
        population=new_population
    until 충분히 적합한 개체가 얻어지거나 충분한 반복 횟수가 지나면
    return 가장 적합한 개체 
}
```

### 8-queen에서 유전자 알고리즘 
* 적합도 값: 서로 공격하지 않는 queen 쌍의 개수 
    만약 모든 queen들이 서로 공격하지 않는다면 28 (7+6+5+...+1=28)
* 적합도 함수: 28-h
    * h: 서로를 공격하는 queen 쌍의 개수 

### 유전자 알고리즘의 장단점 
* 장점: 기존의 방법으로는 해결할 수 없는 제약 조건 만족 문제와 같이 완벽하게 최적의 솔루션이 필요 없는 **어려운 문제**에 종종 사용됨
* 단점
    * **결정론적 알고리즘**이 존재하는 문제에 대해서는 유전자 알고리즘 접근법이 필요 없음 
    * 본질적으로 **확률적 특성**으로 인해 **수행 시간을 예측할 수 없음** (어느 세대에서 끝날지 모르므로..)

### 유전자 프로그래밍(GP:Genetic Programming)
* 유전자 알고리즘의 원리를 프로그래밍에 적용한 것 
* 초기의 랜덤 프로그램에 유전자 연산을 적용하여 특정 작업에 적합한 프로그램으로 진화시키는 기술 
* 프로그램이 스스로 자신을 수정하는 기술로서 컴퓨터 코드를 진화시킴 
* 유전 연산자를 사용하여 트리 자체를 변경함 
    * internal 노드: 연산자
    * 단말 노드: 피연산자 구조 
    * c언어나 파이썬 같은 코드에는 적용할 수 없고 LISP 언어(하나의 큰 트리 구조로 되어 있는 함수형 언어)가 적합
        * (*(+ A B) C)
1. 초기 세대를 랜덤하게 생성
2. 다음을 반복
    * 현재 세대의 각 프로그램을 주어진 데이터 집합에 대하여 평가한 후 적합도를 계산
    * 현재 세대 안의 프로그램을 랜덤하게 선택하여 적합도를 비교
    * 3가지 유전자 알고리즘의 연사자 적용
        * 교차 연산
        * 재생산 연산 
        * 돌연변이 연산
    * 결과 세대 안의 모든 프로그램을 평가
3. 기준이 만족될 때까지 반복 

## 핵심 코드 정리 
```python
    def cal_fitness(self):		    # 적합도 계산 
        self.fitness = 0;
        value = 0
        for i in range(SIZE):
            value += self.genes[i]*pow(2, SIZE-1-i)
        self.fitness = value**2
        return self.fitness
```
```python
# 선택 연산
def select(pop):
    max_value  = sum([c.cal_fitness() for c in population])
    pick = random.uniform(0, max_value)     # 0과 max_value 사이의 난수 발생
    current = 0
    print("Fitness 총합 = ", max_value, ",Pick = ", pick)
    
    # 룰렛휠에서 어떤 조각에 속하는지를 알아내는 루프
    for c in pop:
        current += c.cal_fitness()
        if current > pick:
            return c
```
```python
# 교차 연산
def crossover(pop):
    father = select(pop)
    mother = select(pop)
    index = random.randint(1, SIZE - 1)     # 1~4의 인덱스 선택, 슬라이싱에 사용
    child1 = father.genes[:index] + mother.genes[index:] 
    child2 = mother.genes[:index] + father.genes[index:] 
    return (child1, child2)
    
# 돌연변이 연산
def mutate(c):
    for i in range(SIZE):
        if random.random() < MUTATION_RATE:
            if random.random() < 0.5:
                c.genes[i] = 1
            else:
                c.genes[i] = 0
```
```python
while population[0].cal_fitness() < 31*31:
    new_pop = []

    # 선택과 교차 연산
    for _ in range(POPULATION_SIZE//2):     # // 몫 연산자
        c1, c2 = crossover(population)
        new_pop.append(Chromosome(c1))
        new_pop.append(Chromosome(c2))

    # 자식 세대가 부모 세대를 대체함, 깊은 복사 수행 
    population = new_pop.copy();
    print()
    print("** 교차연산 후")
    print_p(population)
    
    # 돌연변이 연산
    for c in population: 
        mutate(c)
    print("** 돌연변이 후")
    print_p(population)
    
    # 출력을 위한 정렬
    print("+++++++++++++++++++++++++++++++++++++++")
    population.sort(key=lambda x: x.cal_fitness(), reverse=True)
    print("세대 번호:", count)
    print_p(population)
    count += 1
    if count > 20: 
        break
```