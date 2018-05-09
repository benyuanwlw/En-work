# Q-Panda
> Revised versions 1.0 by Wang（Try to trasnlate the origin text of QPandaSdk in English）.

![Q-Panda Logo](/img/Q_Panda_logo.png)

Q-Panda SDK is an open source Quantum Software Development Kit based on Quantum Cloud Service launched by the Origin Quantum Computing Company. The user can develop a quantum program executed in the cloud based on this development kit. Q-Panda use the C++ language as the classical host language and supports the quantum language written in [QRunes](/QRunes/README.md).

Currently, Q-Panda supports up to 32-qubits simulations. It supports two modes of operation: local simulation and cloud simulation. Q-Panda provides an executable command-line program that controls the loading, running and readout of the quantum program. Additionally, Q-panda SDK provides a set of APIs for users to customize the required function.

[Q-Panda Introduction](/QPandaSDK/README.md)

# QRunes

QRunes is a quantum computing instruction set developed by the Origin Quantum computing company.

See details in [QRunes Introduction](/QRunes/README.md)

# QRunes Generator

The QRunes Generator is a C++ library that supports the generation of QRunes instructions as function calls.


See details in [QRunes Generator Introduction](/QRunesGenerator/README.md)

## License

[Apache License 2.0](LICENSE)


## 2. QPandaSDK Interface Introduction

- **loadFile** function

&nbsp;&nbsp;&nbsp;&nbsp;The role of the **loadFile** function is to load and analyze the quantum program written by the user. *loadProgramError* is returned if there is a syntax error during program execution. Otherwise, returns 'ErrorNone'
