# Introduction

QRunes是由本源量子推出的量子指令集。量子指令集是一种运行在量子计算机上的一组基本指令，它可以直接控制量子计算机的运行。QRunes可以从一个很低级的层次直接描述量子程序、量子算法，它的地位类似于经典计算机中的硬件描述语言或者汇编语言。

QRunes is a set of quantum instructions introduced by the origin quantum computing company. A quantum instruction set is a set of basic instructions that run on a quantum computer, which can directly control the quantum computer. QRunes can directly describe quantum programs and quantum algorithms from the underlying hardware level. It is similar to hardware description language or assembly language in the classical computer.


QRunes是由本源量子推出的量子指令集。量子指令集是一种运行在量子计算机上的一组基本指令，它可以直接控制量子计算机的运行。QRunes可以从一个很低级的层次直接描述量子程序、量子算法，它的地位类似于经典计算机中的硬件描述语言或者汇编语言。

In particular, QRunes is designed to manipulate the behavior of a quantum computer directly. Given a QRunes program, only the actions required to operate are included. That is, this is a set of instructions that does not contain any logical decision, whether Classical Data, Classical Control, Quantum Data or Quantum Control. It should be encapsulated in a higher level quantum language since the variable systems not available in QRunes.

#  Basic Syntax

The syntax of QRunes is very straightforward, only the simple form of "instruction + parameter-list" is adopted. A simple

example of QRunes is as follows:

```
% Comment: QRunes Example1
QINIT 2
% Define and initialize 2 qubit
CREG 2
% Define 2 classical register (boolean value) for saving measurement result
H 0
CNOT 0,1
% Perform a series of quantum logic gates
MEASURE 0,$0
MEASURE 1,$1
% Perform measurement, results will be saved and export.
```

Explanation

    %comment

Beginning with "%" is the Comment, the program automatically skips this line when runs here. Similar to "//" in C language.

	QINIT 2

Define two qubits and initialize them to the state |0〉. In QRunes, the first line must define the number of the qubits(except for comment). Specifically, this line is hidden in the page's QRunes Editor by default. When the user click "Run" buttom，the system will automatically add QINIT 2 to the first line.


	CREG 2

定义2个经典寄存器。一个量子程序中第二行（除注释之外）必须显式定义经典寄存器数。经典寄存器是布尔型变量，它是一个经典计算机中的值，并且被初始化到0。经典计算机被用于控制量子计算机的运行，换言之，量子计算机无法脱离经典计算机单独运行。在量子计算机运行时，所有的测量值都会被保存到经典计算机上并且导出。特别地，在网页的量子程序编辑器中，这一行也是隐藏的，在点击“运行”后，这一行定义将被自动附带到程序的第二行。


GREG 2 defined two classical registers, similar to QINIT and placed behind it(except for comment)，which is essential in a quantum program. The classical register is a Boolean variable, which is a classical value and is usually initialized to 0. The quantum computing process must rely on classical computers for assisted control, which means that the operation of quantum computers must rely on classical computers so far. Intuitively, when the quantum computer is running, all measured values are saved to the cloud with classical information and then passed to the user. Specifically, this line is hidden in the page's QRunes Editor by default. Whe
n the user click "Run" buttom，the system will automatically add GREG 2 to the second line.






	H 0

单比特Hadamard门作用在0号量子比特上。量子比特的编号是从0开始的，和一般的程序语言类似。QRunes指定操作的量子比特都是用整型变量描述的。定义了n个比特，合法的整型变量的范围即为0~n-1。




H 0 represents a Hardamard gate acting on qubit 0. Like the general programming language, the qubit index starts from 0 in QRunes. The qubits of QRunes operation is described by an integer variable. For example, N bits are defined, then the range of valid integer variables is 0~N-1.


	CNOT 0,1

两比特CNOT门作用在0,1上。特别要指出的是，对于控制门来说，最右的变量是受控的目标比特。这个原则对TOFFOLI门同样成立。

The CNOT gate acts on the qubits 0,1. In particular, for a control gate(CNOT), the rightmost variable is the controlled target qubit (0 in the case). This principle also holds for the TOFFOLI gate. More details, please refer to the rules of [Quantum Gate](https://en.wikipedia.org/wiki/Quantum_logic_gate).


	MEASURE 0,$0

测量0号量子比特，结果保存到第0个经典寄存器上。$x指定了第x个经典寄存器的位置。



MEASURE 0, $0 indicates to measure 0-th qubit, and the result is saved in the 0-th classical register. $x designates the location of the x-th classical register.


---

# 指令集
# Instruction Set

## 初始化
## Initialization

### **QINIT**


解释：量子态的初始化操作，输入参数为量子比特数目，功能是将所有量子比特置为|0⟩态；

Explanation: The initialization operation of the quantum state, the parameter is the number of qubits,and the role of QINIT is to initialize all the qubits to |0⟩;

Example：

```
QINIT 5
% 定义5个量子比特，初态置为|00000>。
% Defined 5 qubits and initializing the initial state to |00000>，the corresponding index for each qubit is 0,1,2,3,4.
```


### **CREG**
解释：定义保存测量结果的经典寄存器的个数，输入参数为要定义的经典寄存器的个数。经典寄存器的数目一般与最后测量的量子比特数目一致。



Explanation: Define the number of registers (classical information) for saving the measurement results. The input parameters are the number of classical registers to be defined. In general, the number of classical registers always agrees with the number of qubits measured.

Example：
```
CREG 2
% 定义两个经典寄存器
% Defined two classical registers, $0 and $1.
```
## 单比特门
## Single Qubit Gate
### **H / HADAMARD**

解释：单比特Hadamard门，输入参数为目标量子比特序号，功能是对目标量子比特执行Hadamard操作。

Explanation: Single qubit Hadamard gate, the input parameter is the index of the target qubit, the role of HADAMARD is to perform Hadamard operation on the target qubit.

Example：
```
H 4
%对编号为4的量子比特执行Hadamard操作
% The Hardamard gate acts on 5-th qubit.
```

### **RX / RY / RZ**

解释：单比特RX/RY/RZ门，输入参数为目标量子比特序号n和弧度制表示的角度θ，功能是对n号量子比特执行相应的操作；RX/RY/RZ分别表示在Bloch球上绕X/Y/Z轴逆时针旋转θ。


Explanation: Single qubit RX/RY/RZ gate, The input parameters are the target qubit and angle θ represented by the radians. The role of RX/RY/RZ is to perform corresponding operation on the n-th qubit;

 RX/RY/RZ represents the operation of rotating counterclockwise rotation(θ) of the X/Y/Z axis on the [Bloch sphere](https://en.wikipedia.org/wiki/Bloch_sphere), respectively.


例子：
```
RY 3,”3.141592653”
% 对3号量子比特绕Bloch球的Y轴旋转了π
%  The 3-th qubit Rotate π around the Bloch sphere's Y-axis.
```


### **NOT**
解释：单比特NOT门，即量子非门，输入参数为目标量子比特序号，功能是目标量子比特执行NOT操作。

Explanation: A single qubit NOT gate, that is, a quantum NAND gate. The input parameter is the target qubit index, and the role of *NOT* gate performs a NOT operation on the target qubit.

Example:
例子：
```
NOT 1
%1号量子比特执行NOT操作
Perform a NOT operation on 2-th qubit.
```

## 两比特门 Two Qubit Gate.
### **CNOT**
解释：两比特CNOT门，输入参数为控制量子比特序号，目标量子比特序号，功能是对这两个量子比特执行CNOT操作。

Explanation: The CNOT, or [Controlled-NOT gate](https://en.wikipedia.org/wiki/Controlled_NOT_gate), is associated with two qubits, The role of this gate is performed CNOT operations on those two qubits. The input parameters are the index of control qubit and target qubit.


Example：
```
CNOT 3,2
% 3号量子比特为控制量子比特，2号量子比特为目标量子比特，对其执行CNOT操作。
% 4-th qubit is control qubit, and 3-th qubit is target qubit.
```

### **ISWAP**
解释：两比特ISWAP门，输入参数为两个量子比特的序号，功能是执行ISWAP操作

ISWP联系了两个量子比特，它们被执行了Is操作。

Explanation: The ISWAP gate associated with two qubits, which are performed by ISWAP operation. THe parameter is the index of two target qubits.

Example：
```
ISWAP 2,3
% 对2号量子比特和3号量子比特执行ISWAP操作
% Performing ISWAP operation on qubit 3-th and 4-th.
```

### **SQISWAP**
解释：两比特SQISWAP门，输入参数为两个量子比特的序号，功能是执行SQISWAP操作
两比特SQISWAP门，它的功能是执行SQISWAP操作.

Explanation: Two qubits SQISWAP gate performs SQISWAP operations. The parameters
 are the index of two target qubits.

Example：
```
SQISWAP 2,3
%对2号量子比特和3号量子比特执行SQISWAP操作
Performing SQISWAP operation on qubit 3-th and 4-th.
```

## Three qubits gate.
### **TOFFOLI**
解释：三比特TOFFOLI门，输入参数为两个控制量子比特和一个目标量子比特的序号，功能是执行TOFFOLI门操作。

Explanation: The **TOFFOLI** gate associated with three qubits,and perform **TOFFOLI** operation on each qubit. The parameter are the index of two control qubits and the target qubit.



例子：
```
TOFFOLI 1,2,3
%1号和2号量子比特为控制量子比特，3号量子比特为目标量子比特，对这三个量子比特执行TOFFOLI门操作

% 2-th and 3-th qubits are control qubits, and 4-th qubit is the target qubit.
```

## 特殊控制语句
## Special Control Statement

### **CONTROL & ENDCONTROL**

解释：CONTROL控制语句，起始标识是CONTROL,终止标识是 ENDCONTROL,输入参数是量子比特序号n，功能是在CONTROL和ENDCONTROL之间的语句都是受控操作，控制量子比特是n好量子比特。CONTROL&ENDCONTROL语句之间可嵌套CONTROL&ENDCONTROL语句，对应规则是ENDCONTROL对应的CONTROL是离其最近的CONTROL，CONTROL与ENDCONTROL一一对应。

Explanation: The *CONTROL* statement, start identifier is *CONTROL* and end identifier is *ENDCONTRO*. The function is that the statements between CONTROL and ENDCONTROL are controlled, and the control qubit is the n-th qubit.

The input parameter is the qubit index n. Moreover, CONTROL & ENDCONTROL statements can be nested between CONTROL&ENDCONTROL statements, the key point is that they have to be effective in pairs.

Example：
```
CONTROL 2
H 1
CNOT 1,3
RX 4,”3.141592653”
ENDCONTROL 2

% 2号量子比特为控制量子比特，对中间三条语句执行控制操作，即实现受控Hadamard操作，受控CNOT操作，受控RX操作

% The 3-th qubit is the control qubit. The control statement performs control operations on the middle three codes, namely, controled Hadamard operation, controled CNOT operation and RX operation.

```

### **DAGGER & ENDDAGGER**
解释：DAGGER控制语句，起始标识是DAGGER,终止标识是 ENDDAGGER,无输入参数，功能是对DAGGER与ENDDAGGER之间的语句代表的总的操作矩阵M进行转置共轭得到M^†，然后将M^†作用在态空间上，具体实现细节为将DAGGER与ENDDAGGER之间的语句顺序颠倒，然后每个操作变为其转置共轭操作。DAGGER & ENDDAGGER语句也可以嵌套，对应方式CONTROL&ENDCONTROL一致。


Explanation: The *DAGGER* control statement, start identifier is *DAGGER*, and ending by *ENDDAGGER*. The role of this control statement is to perform transpose conjugate on the total circuit operation matrix M between DAGGER and ENDDAGGER to obtain M^†, and then apply M^† to the state space.

The specific implementation details are to reverse the order of statements between *DAGGER* and *ENDDAGGER*, and then perform conjugate transpose to each operations. The DAGGER & ENDDAGGER statement can also be nested, consistent with CONTROL&ENDCONTROL. By the way, there is no parameters need.





Example:

```
DAGGER
H 1
H 4
DAGGER
% DAGGER语句嵌套
% DAGGER statement nesting.
RY 2,” 1.57079633”
TOFFOLI 3,4,2
ENDDAGGER
CNOT 2,3
ENDDAGGER
%具体实现为先对里层DAGGER语句中的操作执行转置共轭操作，将里层DAGGER语句去掉，然后对外层DAGGER语句执行转置共轭操作。

% The specific priority is from the inside to the outside, first perform the transpose operation to the inner layer, and remove the inside DAGGER statement, then perform a transposed conjugate operation to the outer DAGGER statement.

```

### **RESET**
解释：重置操作，输入参数为目标量子比特序号，功能为将目标量子比特强行置为|0⟩态，然后对整个态空间进行归一化。


Explanation:
The *RESET* operation is to forcibly set the target qubit to the state |0⟩ and then normalize the entire state space. The input parameter is the index of the target qubit.



Example：
```
RESET 2
%将2号量子比特置为|0>态。
% set the 3-th qubit to state |0>.
```

## Measurement
### **MEASURE**

解释：测量操作，输入参数为目标量子比特序号和保存测量结果的经典寄存器序号，功能是对目标量子比特进行测量并将测量结果保存在对应的经典寄存器里面，测量后再将整个态归一化。


The Measurement operation is to measure the target qubit and save the result in the corresponding classical register, and the whole state will be normalized after the measurement. The input parameters are the index of the target qubit and the classical register(save the measurement result).

例子：
```
MEASURE 3,$0
% 对3号量子比特进行测量操作，将测量结果保存到0号经典寄存器中。
& Performing measurement on 4-th qubit, and save the result in classical register $0.
```

### **PMEASURE**

解释：联合测量操作，输入参数为一连串要测量的量子比特序号，功能是计算出输入的目标比特构成的子空间所有态的概率。这个操作只是算出概率，并没有改变量子态。PMEASURE语句仅能放置在程序的最后一行，它只是一个量子仿真的功能，并不实际改变量子态。



Explanation: The PMeasure is a joint measurement operations that is to calculate the probability of all states in a subspace formed by the input target qubits. PMeasure operation just print the probability distribution and does not change the initial state. In general, the PMEASURE operation is only a simulation process and always placed in the last line of the program. That is, the parameter is a series of qubit's ordianl index such that in example.


Example:
```
PMEASURE 2,3,6,7
%计算2,3,6,7号量子比特构成的16个态的概率，即2,3,6,7号量子比特处于态|0000>,|0001>……|1111>的概率。

% Calculate the probability distribution of 16 states |0000>,|0001>……|1111> formed by 3-th,4-th,7-th and 8-th qubits.



```
