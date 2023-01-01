# Parallel-Processing-OCR

Parallel processing can increase the number of tasks done by your program which reduces the overall processing time.

- For parallelism, the main problem is divided into sub-problems where each sub-problem/sub-unit is independent of each other.

- Ways to Handle parallel programs 
    1. Shared Memory: Sub-units share the same memory and no need to handle communication.
    2. Distributed Memory: Each process is totally seperate & has own memory.Communication needs to be handled explicitly.

**Threads** are one of the ways to achieve parallelism. 

**Global Interpreter lock** allows only one python instruction to be executed at a time.

GIL limitation can be completely avoided by using processes instead of thread. Using processes have few disadvantages such as less efficient inter-process communication than shared memory, but it is more flexible and explicit.

**Multiprocessing for parallel processing**

Using the standard **multiprocessing** module, we can efficiently parallelize simple tasks by creating child processes. This module provides an easy-to-use interface and contains a set of utilities to handle task submission and synchronization.



**Process and Pool Class**

# Process class

By subclassing ***multiprocessing.process***, you can create a process that runs independently.

```python 

import multiprocessing
import time

class OCRParallel(multiprocessing.Process):
  def __init__(self, id):
    super(OCRParallel, self).__init__()
    self.id = id

  def run(self):
    print('Invoked run()')
    time.sleep(1)
    print(f'Process ID is {self.id}')
    
if __name__ = '__main__':
  p = OCRParallel(0)
  p.start()
  p.join()
  p = OCRParallel(1)
  p.start()
  p.join()

```

# Pool class

Pool class can be used for parallel execution of a function for different input data. The multiprocessing.Pool() class spawns a set of processes called workers and can submit tasks using the methods apply/apply_async and map/map_async. For parallel mapping, you should first initialize a multiprocessing.Pool() object. The first argument is the number of workers; if not given, that number will be equal to the number of cores in the system

```python 

pool = multiprocessing.Pool()# creates processes equal to the number of cores
pool = multiprocessing.Pool(processes = 2)

def square(x):
  return x*x

res = pool.map(square, [2,3,4,6])
res_async = pool.map_async(square, [2,3,4,6])

res_async = [pool.apply_async(square,args= (i,) ) for i in range(100)]
res = [r.get() for r in res_async]
print(res)

```
# Parallelizing PyTesseract OCR using multiprocessing library

Install ocr dependencies 

```linux 
!sudo apt install tesseract-ocr
!pip install pytesseract
```

```python 
import multiprocessing

#get a sample dataset of image files
sample = rvl_cdip['train'][:100]['image']

def ocr(image):
  return pytesseract.image_to_string(image) 
  
pool = multiprocessing.Pool()


res_async = [pool.apply_async(ocr,args= (i,) ) for i in sample]
res = [r.get() for r in res_async]
```
