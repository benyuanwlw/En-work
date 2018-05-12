# QPandaSDK
## 1.QPandaSDK Introduction


Q-Panda SDK是由本源量子推出的，基于量子云服务的，开源的量子软件开发包。用户可基于此开发包开发在云端执行的量子程序。Q-Panda使用C++语言作为经典宿主语言，支持以QRunes书写的量子语言。

目前，Q-Panda支持本地仿真运行模式和云仿真运行模式，最高可支持到32位。Q-Panda提供了一个可执行的命令行程序，通过指令控制量子程序的加载、运行和读出。另外，Q-Panda提供了一组API，可供用户自行定制功能。

&nbsp;&nbsp;&nbsp;&nbsp;Q-Panda SDK is an open source Quantum Software Development Kit based on Quantum Cloud Service launched by the Origin Quantum Computing Company. The user can develop a quantum program executed in the cloud based on this development kit. Q-Panda use the C++ language as the classical host language and supports the quantum language written in QRunes.

&nbsp;&nbsp;&nbsp;&nbsp; Currently, Q-Panda supports up to 32-qubits simulations. It supports two modes of operation: local simulation and cloud simulation. Q-Panda provides an executable command-line program that controls the loading, running and readout of the quantum program. Additionally, Q-panda SDK provides a set of APIs for users to customize the required function.

## 2. QPandaSDK Interface Introduction

- **loadFile** function

loadFile函数的作用是加载并解析用户编写的量子程序，如果程序有语法错误则返回loadProgramError，否则返回qErrorNone。其输入参数为需要解析的量子程序的路径。输出参数为解析的量子程序需要解析的量子bit数。

&nbsp;&nbsp;&nbsp;&nbsp;The role of the **loadFile** function is to load and analyze the quantum program written by the user. *loadProgramError* is returned if there is a syntax error during program execution. Otherwise, returns *qErrorNone*.

Table 2.1 &nbsp;&nbsp; **loadFile** function
---
    /*************************************************************************
    Name:        loadFile
    Description: load quantum program
    Argin:       sFilePath：quantum program file path
    Argout:      Qnum      ：    quantum bit num
    return:      qerror
    *************************************************************************/
    QError loadFile(const string &sFilePath,int &Qnum);

-  **setComputeUnit** function

setComputeUnit函数的作用是设置使用哪种方式模拟量子计算，用户有两种选择：CPU模式和GPU模式。如果设置失败则返回setComputeError，否则返回qErrorNone。函数的输入参数为计算单元的类型。

&nbsp;&nbsp;&nbsp;&nbsp;The role of the **setComputeUnit** function is to set the way to simulate quantum computing. There are two choices for user: CPU mode or GPU mode. If user settings fail then returns *setComputeError*, otherwise returns *qErrorNone*, and the input parameter of the function is the type of the calculation unit.


Table 2.2&nbsp;&nbsp;**setComputeUnit** function
---
    /*************************************************************************
    Name:        setComputeUnit
    Description: set compute unit,you have two options:CPU or GPU.you can not
    choose again when you have already selected a compute unit
    Argin:       iComputeUnit: the type of compute unit
    Argout:      None
    return:      qerror
    *************************************************************************/
    QError QParserProgram::setComputeUnit(int iComputeUnit);


- **run** function

&nbsp;&nbsp;&nbsp;&nbsp; run函数的作用是运行解析的量子程序。量子程序根据其测量模式分类两种：Measure和PMeasure模式。Measure模式即为蒙特卡洛模式，由于目标量子在测量前，其状态是不确定的，所以该模式需要运行多次才能得到各种情况的概率。PMeasure模式即为概率模式，该模式会统计出每个目标量子各种可能的状态并统计其概率，PMeasure模式的程序只会运行一次。如果运行不成功则返回runProgramError，否则返回qErrorNone。函数的输入参数为需要重复运行的次数。

&nbsp;&nbsp;&nbsp;&nbsp; The role of the **run** function is to run a parsed quantum program. In Origin Q, a quantum program has two classifications based on their measurement modes: Measure and PMeasure modes. Here, the Measure mode is Monte Carlo model, and this mode needs to be run multiple times to get the probability distribution of various state collapses since the target qubit is indeterminate before the measurement.
The PMeasure mode is a probabilistic model, which can count the results of the possible collapse of the target quantum state and print its probability distribution. The difference between Measure and PMeasure mode is that the program runs only once in this case. During program execution, if the run was unsuccessful then returns *runProgramError*, otherwise returns *qErrorNone*. In **run** function, the input parameter of the function is the number of times that the measurement needs to run repeatedly.


Table 2.3 &nbsp;&nbsp;**run** function
---
    /*************************************************************************
    Name:        run
    Description: run quantum program
    Argin:       iRepeat    quantum program repeat
    Argout:      None
    return:      qerror
    *************************************************************************/
    QError run(int iRepeat);

- **getResult** function

 getResult函数的作用是获取量子程序的运行结果。如果获取不成功则返回getResultError，否则返回qErrorNone。函数的输入参数为运行后的结果。

&nbsp;&nbsp;&nbsp;&nbsp;The role of the getResult function gets the running results of a quantum program. If the retrieval was unsuccessful then returns *getResult*, or *qErrprNone* otherwise. Moreover, the input parameter of this function is the result of the running.


Table 2.4&nbsp;&nbsp;**getResult** function  
---
    /*************************************************************************
    Name:        getResult
    Description: get quantum program result
    Argin:       None
    Argout:      sResult   result string
    return:      qerror
    *************************************************************************/
    QError getResult(STRING & sResult);

- getQuantumState function

    getQuantumState函数的作用是获取量子程序运行后的整个量子体系所有分量的复振幅。如果获取不成功返回getQStateError，否则返回qErrorNone。函数的输入参数为string类型的所有分量的复振幅。

&nbsp;&nbsp;&nbsp;&nbsp;The role of the getQuantumState function is to obtain the complex amplitude of all components of the entire quantum system after the given quantum program runs. *getQStateError* if unsuccessful, or *qErrprNone* otherwise. The input parameter of the function is the complex amplitude of all components (parameter type is string).




Table 2.5&nbsp;&nbsp;getQuantumState function  
---
    /************************************************************************
    Name:        getQuantumState
    Description: get quantum program qstate
    Argin:       None
    Argout:      sState   state string
    return:      qerror
    ************************************************************************/
    QError QParserProgram::getQState(STRING & sState);


## 3. QPandaSDK Compilation Introduction

QPandaSDK分为两个部分：QPandaAPI以及基于QPandaAPI搭建的量子程序执行系统。

&nbsp;&nbsp;&nbsp;&nbsp; QPandaSDK has two parts: QpandaAPI, and a quantum program execution system based on QPandaAPI.

### QPandaAPI Compilation

QPandaAPI可使用makefile编译和Visual Studio编译。

&nbsp;&nbsp;&nbsp;&nbsp; QPandaAPI can be compiled using Makefile or Visual Studio compiler.

- makefile Compilation

使用makefile编译比较简单，直接执行make命令即可。在此之前用户需要验证本机是否已经安装cuda，如果未安装则无法使用GPUEmulator库。

&nbsp;&nbsp;&nbsp;&nbsp; Compiling with Makefiles is straightforward, and the user can execute the **make** command directly. Before using the makefile, the user needs to verify that *Cuda* is installed in the running environment. Otherwise, the GPUEmulator library cannot be used without installation.

- Visual Studio Compilation

&nbsp;&nbsp;&nbsp;&nbsp;There are several steps to using the Visual Studio compiler:

- Create.

- Loading the subproject of QPandaAPI、QRunesParser、QuantumInstructionHandle and GPUEmulator.；

- Compile QuantumInstructionHandle、GPUEmulator、QRunesParser and QPandaAPI in order(Order from the first).



### The Execution Compilation of Quantum Program

量子程序执行系统需要使用Visual Studio编译。本系统由于使用了一些公共库，所以只支持64位。量子程序执行系统在编译前需要准备以下环境：

&nbsp;&nbsp;&nbsp;&nbsp; Our system only supports Visual Studio 64-bit compilation since we use some public libraries. Before compiling the quantum program, users need to prepare the following environment:

- python 2.7.14

&nbsp;&nbsp;&nbsp;&nbsp;量子程序执行系统使用Visual Studio编译分为以下步骤：

- 除ConsoleApplication外，其他项目设置为dll动态链接库;

- 设置解决方案平台为x64;

- 依次编译QuantumInstructionHandle、GPUEmulator、QRunesParser、QPandaAPI、QuantumCloudHTTP、QuantumCommandControl库与ConsoleApplication应用程序。


- python 2.7.14

&nbsp;&nbsp;&nbsp;&nbsp;Compilation has the following steps(Visual Studio):

- Except *ConsoleApplication*, other projects are set as dynamic link library(DLL);

- Setting the solution platform to x64;

- Compile *QuantumInstructionHandle*、*GPUEmulator*、*QRunesParser*、*QPandaAPI*、*QuantumCloudHTTP* and *QuantumCommandControl* libraries sequentially, and the application *ConsoleApplication* .

## 4. QPandaAPI Usage Example.

Table 4.1&nbsp;&nbsp; Examples
---
```
  int main(int argc, char *argv[])
  {
      string sResult,sQState;
      string sFilePath = "test.txt";
      int    iQnum     = 0;
      int    iRepeat   = 100;

      QPanda::QParserProgram  qParserProg;
      qParserProg.loadFile(sFilePath);
      qParserProg.setComputeUnit(CPU);
      qParserProg.run(iRepeat);
      qParserProg.getResult(sResult);
      qParserProg.getQuantumState(sQState);

      cout << "result:\n" << sResult << endl;
      cout << "QState:\n" << sQState << endl;

      getchar();
      return 0;
  }
```

## 5. Examples of Quantum Program Execution.

&nbsp;&nbsp;&nbsp;&nbsp;使用量子程序执行系统前需要先进行以下步骤：

&nbsp;&nbsp;&nbsp;&nbsp; Before using the quantum program execution system, the following steps need to be performed:



- 从 http://www.qubitonline.cn/ 中注册并获取APIkey；

- 把APIkey保存在文件中，命名文件为key，并把key文件保存在ConfigFile文件夹中；

- 把ConfigFile文件夹复制到ConsoleApplication.exe同一文件夹下；

- 安装DepInstalPackage中python 2.7.14。


- Register and get the APIkey from http://www.qubitonline.cn/.

- Save the APIkey and name the file as key, and save this file in the ConfigFile folder.

- Keep the configFile folder and ConsoleApplication.exe file under the same folder.

- Install Python 2.7.14 in DepInstalPackage.

&nbsp;&nbsp;&nbsp;&nbsp;量子程序执行系统分为本地仿真运行模式和云仿真运行模式，可在启动程序时使用mode命令选择运行模式。
&nbsp;&nbsp;&nbsp;&nbsp; The quantum program execution system is divided into two types: the local simulation mode and the cloud simulation mode, and use the mode command to select the corresponding simulator when the program starts.

---
    ConsoleApplication.exe mode [mode]
    
- mode 1 represent the Local Simulation Mode；
- mode 2 represent the Cloud Simulation Mode

### Local Simulation Mode Run Command

&nbsp;&nbsp;&nbsp;&nbsp;The local simulation run mode command is as follow:
- **load** : Loading local quantum program file.

---
    load [path]

- **run** : running a quantum program that has been loaded.

---
    run -n [repeat] -gpu -f -o -b
    -n    repetitions
    -gpu  select GPU to simulate quantum computing（Default is CPU）
    -f    Save the quantum state
    -o    Save the run results
    -b    Save the quantum state in binary(0 or 1).


 ### The cloud simulation model

 &nbsp;&nbsp;&nbsp;&nbsp;The cloud simulation mode run commamd are shonw below：

 - submit :Submit local quantum program file.

---
    submit [path] -n [repeat]
    -n    repetitions
    

- **tasklist**  Get user submitted quantum program TaskID.
- **getresult**  Get the running results of the quantum program.

---
    getresult [TaskID]

## 6. Using The Python to Call The QPandaAPI Classes


- 在QPandaSDK文件夹下执行make命令，生成libQRunesParser.so、libQuantumInstructionHandle.so动态库；

- 在QPandaAPI文件夹下执行make -f Makefile.QPandaAPI-swig命令，生成_QPandaAPI.so库；

- 复制QPandaAPI.py到linuxlib文件夹下。 


- Execute the **make** commamd to generate *libQRunesParser.so* and *libQuantumInstructionHandle.so* dynamic libraries under the QPandaSDK folder.

- Execute the **make -f Makefile.QPandaAPI-swig** commamd to generate **_QPandaAPI.so** library under the QPandaAPI folder.

- Copy *QPandaAPI.py* to *linuxlib* folder.
