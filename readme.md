HW03
===
This is the hw03 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw03 dir, use:

	* `make` to build.

	* `make clean` to clean the ouput files.

4. Extract `gnu-mcu-eclipse-qemu.zip` into hw03 dir. Under the path of hw03, start emulation with `make qemu`.

	See [Lecture 02 ─ Emulation with QEMU] for more details.

5. The sample is a minimal program for ARM Cortex-M4 devices, which enters `while(1);` after reset. Use gdb to get more details.

	See [ESEmbedded_HW02_Example] for knowing how to do the observation and how to use markdown for taking notes.

# Build Your Own Program

1. Edit main.c.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C Standard library functions like `printf`, `malloc`, etc.

# HW03 Requirements

1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)

[Lecture 02 ─ Emulation with QEMU]: http://www.nc.es.ncku.edu.tw/course/embedded/02/#Emulation-with-QEMU
[ESEmbedded_HW02_Example]: https://github.com/vwxyzjimmy/ESEmbedded_HW02_Example

--------------------

- [] **If you volunteer to give the presentation next week, check this.**

--------------------

```cpp=
int plus(int a, int b,int c,int d,int e){ return a+b+c+d+e; }
void reset_handler(void)
{
	plus(1,2,3,4,5);
	while (1)
		;
}
```
實驗發現當引數為五個時系統自動將最後一個引數（5）先是放進r3後再存入stack兒其他剩下的四個引數(1,2,3,4)就分別放進r0到r3

![](https://i.imgur.com/xoSGyuf.png)
![](https://i.imgur.com/MXIgsfB.png)


```cpp=
int plus(int a, int b,int c,int d){ return a+b+c+d; }
void reset_handler(void)
{
	plus(1,2,3,4);
	while (1)
		;
}

```

實驗發現當引數為四個時系統自動將四個引數(1,2,3,4)分別放進r0到r3

![](https://i.imgur.com/3U9YDPn.png)
![](https://i.imgur.com/RFLU1vY.png)

查閱ARM AAPCS資料後的結論是資料傳送到暫存器R0～R3中。如果引數多於4個，將剩餘的字資料傳送堆疊中
