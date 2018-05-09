# QPandaSDK
## 1.QPandaSDK Introduction

&nbsp;&nbsp;&nbsp;&nbsp;Q-Panda SDK is an open source Quantum Software Development Kit based on Quantum Cloud Service launched by the Origin Quantum Computing Company. The user can develop a quantum program executed in the cloud based on this development kit. Q-Panda use the C++ language as the classical host language and supports the quantum language written in QRunes.

&nbsp;&nbsp;&nbsp;&nbsp; Currently, Q-Panda supports up to 32-qubits simulations. It supports two modes of operation: local simulation and cloud simulation. Q-Panda provides an executable command-line program that controls the loading, running and readout of the quantum program. Additionally, Q-panda SDK provides a set of APIs for users to customize the required function.

## 2. QPandaSDK Interface Introduction

- **loadFile** function

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


- **run** 函数

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


## 3. QPandaSDK Compilation introduction

&nbsp;&nbsp;&nbsp;&nbsp; QPandaSDK has two parts: QpandaAPI, and a quantum program execution system based on QPandaAPI.

### QPandaAPI compilation

&nbsp;&nbsp;&nbsp;&nbsp; QPandaAPI can be compiled using Makefile or Visual Studio compiler.

- makefile Compilation

&nbsp;&nbsp;&nbsp;&nbsp; Compiling with Makefiles is straightforward, and the user can execute the **make** command directly. Before using the makefile, the user needs to verify that *Cuda* is installed in the running environment. Otherwise, the GPUEmulator library cannot be used without installation.

- Visual Studio Compilation

&nbsp;&nbsp;&nbsp;&nbsp;There are several steps to using the Visual Studio compiler:

- Create.

- Loading the subproject of QPandaAPI、QRunesParser、QuantumInstructionHandle and GPUEmulator.；

- Compile QuantumInstructionHandle、GPUEmulator、QRunesParser and QPandaAPI in order(Order from the first).



### The execution compilation of quantum program

&nbsp;&nbsp;&nbsp;&nbsp; Our system only supports Visual Studio 64-bit compilation since we use some public libraries. Before compiling the quantum program, users need to prepare the following environment:

- python 2.7.14

&nbsp;&nbsp;&nbsp;&nbsp;Compilation has the following steps(Visual Studio):

- Except *ConsoleApplication*, other projects are set as dynamic link library(DLL);

- Setting the solution platform to x64;

- Compile *QuantumInstructionHandle*、*GPUEmulator*、*QRunesParser*、*QPandaAPI*、*QuantumCloudHTTP* and *QuantumCommandControl* libraries sequentially, and the application *ConsoleApplication* .

## 4. QPandaAPI usage example.

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

## 5. Examples of quantum program execution.

&nbsp;&nbsp;&nbsp;&nbsp; Before using the quantum program execution system, the following steps need to be performed:

- Register and get the APIkey from http://www.qubitonline.cn/.

- Save the APIkey and name the file as key, and save this file in the ConfigFile folder.

- Keep the configFile folder and ConsoleApplication.exe file under the same folder.

- Install Python 2.7.14 in DepInstalPackage.

&nbsp;&nbsp;&nbsp;&nbsp; The quantum program execution system is divided into two types: the local simulation mode and the cloud simulation mode, and use the mode command to select the corresponding simulator when the program starts.

---
    ConsoleApplication.exe mode [mode]
    
- mode 1 represent the Local Simulation Mode；
- mode 2 represent the Cloud Simulation Mode

### Local simulation mode run command

&nbsp;&nbsp;&nbsp;&nbsp;The local simulation run mode command is as follow:
- **load** : Loading local quantum program file.

---
    load [path]

- run : running a quantum program that has been loaded.

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

## 6. Using The Python to Call The QPandaAPI Class


- Execute the **make** commamd to generate *libQRunesParser.so* and *libQuantumInstructionHandle.so* dynamic libraries under the QPandaSDK folder.

- Execute the **make -f Makefile.QPandaAPI-swig** commamd to generate **_QPandaAPI.so** library under the QPandaAPI folder.

- Copy *QPandaAPI.py* to *linuxlib* folder.

















