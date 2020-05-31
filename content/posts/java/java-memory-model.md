+++ 
draft = false
date = 2020-05-31T03:49:42+05:30
title = "Java Memory Model"
slug = "" 
author = "Akhilesh Dubey"
tags = ["java","heap","stack"]
categories = ["java"]

+++

- Heap: Dynamic memory area and Shared among all threads of an application. When full, We get java.lang.OutOfMemoryError.

- Young Generation Area:
    - Eden Space: Newly created Objects goes in Eden Space.
    - Survivor Space: ( Two survivor space s0 & s1): When Eden space is filled, Miner GC is performed and all survivor object moved into survivor space.
    - Old/Tenured Generation Area: The remaining objects moved into Old generation which are survivor after many cycle of miner GC. Major GC is performed when Old generation is full.

- Permanent Generation(PermGen): It is not part of Heap Memory area. It contains Application's metadeat i.e. classes and methods's description for JVM.

- Method Area / Class Area: Shared among all threads of an application. Also called PremGen before Java 8, It contains classes structure, constant pool, static method, static variable, constant, method codes and constructor.
- Memory Pool: String pool comes under memory pool area. It can belongs to heap or PermGen area depending upon Memory Manager implementation.
- Runtime Constant Pool: It contains class specific runtime constant of a class and static method, comes under method area.
- Stack: Static memory area and constant area, Generated for each thread, When full, We get StackOverflowError, It keeps method reference, local variables of method including argument variable and reference to local object created inside method.
- Caching: (Byte, Character, Integer, Short, Long): Will Discuss it later.
- Program Counter Register: It is generated for each thread and it stores current instruction executed by thread.
- Native Method Stack: Shared among all threads, Same as stack, but used for native method which are outside of Java.

Changes done in Memory Model of JVM: After Java 8 : PermGen kicked-off and MetaSpace kicked-in

- Class meta-data, interned Strings and class static variables will be moved from the permanent generation to either the Java heap or native memory.
- Java8 implementation move Class meta-data in native heap called MetaSpace and move interned Strings and class statics to the Java heap.
- By default metaspace is unlimited and GC happens after 16 MB. its limit is specified with MaxMetaspaceSize. GC internally maintains what we call as the high-water-mark for the metaspace at which collections for the Metaspace are invoked, and it gets initialized with the value specified with MetaspaceSize. The HWM value is lowered or raised depending upon the free space after a GC.
- The value of MaxMetaspaceFreeRatio determines how much maximum free space should be available after a GC. If the committed space available after a GC is more than MaxMetaspaceFreeRatio value, then the HWM is lowered.
- The code for the permanent generation in the Hotspot JVM will be removed.

Each Thread contains:
- Stack,
- Program Counter(PC) register
- Native Stack

Shared Things among all thread:
- Heap
- PermGen/ Class Area/ Method Area

Each Object contains:
- Monitor
- Wait Set

- Escape Analysis (EA): It is a tool that the JIT compiler used to check the scope of the newly created object and decide weather the object will be placed in a heap or stack.

There are three type of escape states objects:

- NoEscape – When the object is not visible outside of the current method or thread.
- ArgEscape – When the object is passed to a method by argument and is not visible outside of the current method or other threads.
- GlobalEscape – When the object is visible by outside of the method or the thread, e.g when a object is returned from a method or is declared static.

If the object is NoEscape, It will not be kept in Heap area, Instead it will reside in Stack area.

If the object is NoEscape or ArgEscape, JIT compiler can revove the locking and synchronization technique to make it fast.
 e.g. If a StringBuffer is declare inside a method body It will be called NoEscape object, and JIT compile will remove locking and synchronization check on its object.

Creation of PermGen 2006 : More: https://blogs.oracle.com/jonthecollector/presenting-the-permanent-generation

Removal of PermGen 2014 : More: https://blogs.oracle.com/poonam/about-g1-garbage-collector,-permanent-generation-and-metaspace
