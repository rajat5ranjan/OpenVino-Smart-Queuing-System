# Udacity Nanodegree

## Choosing the right hardware Notes
## CPU Specifications (Part 2)

### Testing Performance
As you just saw, there are quite a few factors to consider when looking at CPU specifications, such as:

* Cost
* Performance
* Power requirements
* Ambient temperature
* Lifespan
But when choosing a CPU for an edge system, we can't simply go over this list and pick the CPU based only on the listed specifications. Choosing a system or processor for an edge device is different from choosing a laptop. AI systems are constantly changing, and there is not yet a universally acknowledged performance guide that tells you exactly what processor is optimal based on the specifications alone.

Getting familiar with the specifications will give you some general ideas of the range of devices available, but, ultimately, the best way to choose the processor for your system is to test the performance of your models on multiple devices and see how they perform. One of the key advantages of the DevCloud is that it allows you to test a range of devices before you invest in purchasing any particular hardware.

### Considering Client Needs and Constraints
In addition to testing the performance of different CPUs, you will also want to consider the needs and constraints of the client (or clients) you are working with. The client may have certain requirements, such as tolerance for high ambient temperature. Or they may want to use pre-existing hardware and save money by only upgrading their CPU; in this case, you will need to consider whether the motherboard has a socket that is compatible with the CPU you are going to use.

In some cases, you may find yourself building a system for more than one client, and each client will have their own needs and constraints. In these circumstances, it may make sense to select a CPU that helps make your system more flexible and modifiable.


### Exercise 1 CPU Scenario
Ventilation is a serious concern in an underground coal mine as there is a constant fear of unsafe toxicity levels for mine workers.

The mining company would like to install a system that would enable monitoring of the ventilation and toxicity levels in the mine shafts. Sensors would be placed in the mine shafts themselves, and these sensors would be connected to pre-existing PCs that are located near the entrance. The sensors would detect the air quality and relay this information back to the computers. If low air quality or high toxicity levels are detected, the system would quickly raise an alert so the mine could be evacuated. The ambient temperature at the mine’s entrance ranges from 15° to 40° C and noise levels are 80 to 120 decibels (dB).

The mining company has a budget of $1,000 per PC to upgrade any hardware necessary. Based on the existing hardware, a power consumption of up to 100 watts from the CPU can be supported.

You are the IoT engineer and your job is to identify what hardware might work for this system. One of the possibilities you're considering is the Xeon "Coffee Lake" (E-2176G) Processor. Your goal on this page will be to determine whether this processor meets the requirements of the client.


## Integrated GPU (IGPU)
One of the things we noted was that this processor shows not only four CPUs, but also a GPU. As we mentioned, all Intel processors contain a GPU—and since this GPU is located on the same chip and shares the same memory with the CPUs, it is referred to as an Integrated GPU.

An integrated GPU (IGPU) is a GPU that is located on a processor alongside the CPU cores and shares memory with them.

In this section, we'll take a look at the architecture of Integrated GPUs. The basic building blocks are:

* Execution Units (EUs) are processors optimised for multi-threading. Each EU can run up to seven threads simultaneously.
* A Slice is a collection of 24 EUs. Slices handle computational tasks, such as running inference with OpenVINO.
* The Unslice is the remainder of the IGPU. It's main functions are to support video playback functions.
Note that the decoupling of the slice and unslice is what allows for OpenVINO inference to run on the Execution Units on the slice, while the video playback, if required, is handled by the unslice.

### Key Characteristics of Integrated GPUs
* Configurable Power Consumption. The clock rate for the slice and unslice can be controlled separately. This means that unused sections in a GPU can be powered down to reduce power consumption.
* OpenCL Startup Time. When loading a model, OpenCL uses a just-in-time compiler to compile the code for whatever hardware is being used. With an IGPU, this can lead to significantly longer model load times when compared to the same OpenVINO application running on just a CPU.
* Model Precision and Speed. On IGPUs, the Execution Unit instruction set and hardware are optimized for 16bit floating point data types. This improves inference speed, as we can process twice as many 16bit operands per clock cycle as we can when using 32 bit operands.
* Shared Components. The CPU and IGPU are present on the same die and they share the same system memory, higher-level caches, and memory controller. This reduces memory latency by speeding up data transfer between the two devices.

### Integrated GPUs vs. External GPUs
You should be aware that, generally speaking, external graphics cards will have much higher performance than integrated GPUs. However, this doesn't mean that having an external GPU will always be better. Rather, you should consider the specific needs and constraints of each specific situation. For example, adding a dedicated graphics card to a machine will be an additional expense and may draw significantly more power.


### IGPU and Batch Processing
Another key characteristic of IGPUs is related to the relationship between performance and batch size. Here is an illustration of a typical scenario, comparing the frames per second between a CPU and IGPU when the data is processed in a batch size of 1 vs. a batch size of 32:

Let's consider why this is the case. As we've seen, a CPU typically has 4-8 cores, allowing it to do some degree of parallel processing. In contrast, we saw that an IGPU may have 24 to 72 execution units—in other words, IGPUs generally can handle a much larger number of processes at once. Thus, if our data is divided into batches, then an IGPU can process multiple batches simultaneously, which can sometimes give a significant boost in performance.

Note that this relationship does not always hold true—meaning, not every model will run faster on IGPU when you do batch processing. It will depend on the specific CPU and model in question. To get an idea of the variation that can occur, have a look at this chart that compares the frames per second (FPS) across different devices and with multiple models:

The bottom line is that if you will be processing the data in large batch sizes (e.g., if you are getting a large number of images or frames as input all at once) then IGPU may give you a performance boost. But since this isn't always the case, it's good to test your application on multiple hardware types on the DevCloud to see how the actual performance results compare.

### Exercise: IGPU Scenario
Let us consider a traffic monitoring system situated at Bugis, Singapore. There is a single camera monitoring a single lane at the intersection for the purposes of detecting traffic violations (e.g., speeding). Mr. Tan wants to buy an edge processor that he can attach to the camera. The goal is to perform inference on the frames captured by the camera to detect the license plates of the traffic violators.

The edge processor will be attached to a solar-powered battery, which provides up to 30W of power. Along with this, they have a low budget for this test project and can only afford up to 60 USD for the processor.

Can you suggest a processor for this system?
Ans:
cost of the processor 
power consumptiion of the processor

Mr. Tan has been using this device for the last month and has decided to expand on the project to monitor all lanes in the intersection and increase the reliability of the system. In the expanded system, there are now 10 cameras situated at the intersection. The new system captures 10 images at a time (one form each camera) and delivers them all at once to the processor.

With this additional load, he finds the current system to be insufficient since it takes a long time for inference: about 16 milliseconds to detect the license plate for each image. They need to process each image within 9 milliseconds, so he wants to reduce the current inference time by half. They are also over-budget and want to avoid spending any more of their funding.

Ans:
INtel Atom with integrated HD graphics card

Inference ran
Intel Atom x7-E3950 for your CPU
Intel HD Graphics 505 for your IGPU


### Lesson Review
This lesson was all about CPUs and integrated GPUs. In this lesson, we covered this main topics:

CPUs
Basic CPU concepts
Architecture, specifications, and applications of CPUs
How to use a CPU on the DevCloud

Integrated GPUs
Characteristics and architecture of integrated GPUs
How to use an Integrated GPU on the DevCloud


##  VPU's

### Lesson Overview
This lesson is all about Vision Processing Units (VPUs). In this lesson, we will learn about:

* The Architecture of VPUs
* The characteristics of a particular VPU, the Myriad-X processor
* The Neural Compute Stick 2 (NCS2), which uses the Myriad-X Processor
* * The characteristics of the NCS2
* * How to access the NCS2 on the DevCloud
* OpenVINO's Multi-Device Plugin
* * How to run the same model on multiple devices at once using the Multi-Device Plugin
* * How to use the Multi-Device Plugin on the DevCloud

### Glossary
You can download a PDF containing the main terms used throughout this course here or at any time from the Resources section on the left.

### Introduction to VPUs
In some situations, you may find that you need to upgrade your system in order to improve the performance and run the types of inference you need—but you might not have the budget to fully replace your current hardware.

For example, you might want to upgrade your processor, but discover that in order to install the new processor you would also need to upgrade the motherboard. This could be very expensive, possibly costing you $500 or more per machine.

In this lesson, we'll look at an alternative: VPUs.

`Vision Processing Units (VPUs) are accelerators that are specialized for AI tasks related to computer vision—such as Convolutional Neural Networks (CNNs) and image processing.`

We'll see that VPUs provide a cost-efficient way to add performance to a pre-existing system.

Let's look at an example where a VPU would be a good way to improve the ability of your system to perform inference in an edge system.

VPUs are small, low-cost, low-power devices that can dramatically improve the performance of a system without the need to upgrade the other hardware.

Note that a VPU is an accelerator, meaning it accelerates the performance of the pre-existing CPU. The CPU doesn’t need to be a powerful one, since it will not actually be doing any calculations—we could use a simple Atom Processor or even a Raspberry Pi—but it is needed in order to coordinate the flow of data to and from the VPU.

### Architecture of VPUs
For this course, you don't need to have a deep understanding of how a VPU works, but you should get a general idea of the basic VPU architecture. Intel VPUs consists of the following parts:

* Interface unit
* Imaging accelerators
* Neural compute engine
* Vector processors
* On-chip CPUs

Let's have a look at each of these parts and how they relate to the VPU's performance.

* Interface unit. The interface unit is the part of the VPU that interacts with the host device. This host device could be a CPU or any other processing device. We would train our machine learning models on the host device and then run the inference on the VPU. VPUs are available with a variety of connection types (such as USB 3.1 and Gigabyte Ethernet), making it possible to add a VPU to many different types of pre-existing systems.

* Imaging accelerators. As we said earlier, VPUs are specialized for image processing. One example of this specialization is found in the imaging accelerators, which have specific kernels that are used for image processing operations. These operations range from simple techniques for denoising an image, to algorithms for edge detection.

* Neural compute engine. Modern Intel VPUs, such as the Myriad X, feature a neural compute engine, which is a dedicated hardware accelerator optimized for running deep learning neural networks at low power without any loss in accuracy.

* Vector processors. Vector processors, as the name suggests, are processors that work on a vector or an array of 1D data. They can be contrasted with scalar processors, which often work on single data items. In general, with single data items, instructions are executed in a sequential manner, which increases the time required for execution. In contrast, the vector processors in a VPU can break up a complex instruction and then execute many tasks in a parallel manner.

* On-chip CPUs. VPUs have specialized on-chip CPUs. The Myriad X VPU has two: one used to run the host interface and the other is used for on-chip coordination between the Neural Compute Engine (NCE), the vector processor, and the imaging accelerators.

### Myriad X Characteristics
* Neural compute engine. The Myriad X features a neural compute engine, which is a dedicated hardware accelerator optimized for running deep learning neural networks at low power without any loss in accuracy.

* Imaging accelerators. As we said earlier, VPUs are specialized for image processing. One example of this specialization is found in the imaging accelerators, which have specific kernels that are used for image processing operations. These operations range from simple techniques for denoising an image, to algorithms for edge detection.

* Imaging/Hardware Accelerators. As we mentioned before, VPUs have specialized accelerators for image processing. The Myriad X includes accelerators for functions like hardware encode and decode of H.264 and Motion JPEG, as well as a warp engine for handling fisheye lens, dense optical flow, and stereo depth perception. These accelerators are used in advanced 3D imaging devices, such as Intel’s Realsense 3D cameras.

* On-chip memory. The Myriad X has 2.5 Mbytes of on-chip memory. This is key to minimizing off-chip data movement, which in turn reduces latency and power consumption.

* Vector processors. The Myriad X has sixteen proprietary vector processors known as Streaming Hybrid Architecture Vector Engine (SHAVE) processors. SHAVE processors use 128bit VLIW (Very long instruction word) architecture and are optimized for computer vision workloads.

* Energy consumption. The Myriad X has a very low power consumption of only 1-2 watts.

### Intel Neural Compute Stick 2

We've discussed the architecture, characteristics, and advantages of VPUs, but we haven't yet discussed how you would actually connect one to a pre-existing system. One hardware solution is the Intel Neural Compute Stick 2.

`The Neural Compute Stick 2 (NCS2) is a USB3.1 plug and play removable VPU for AI inferencing.`

Here are the key features of the Intel NCS2:

* VPU. The processor in the NCS2 is the Myriad X VPU.
* Software development kit. With the integration of OpenVino Toolkit the Intel NCS2 offers pre-trained models to be run on the stick. This allows ease in the use of the hardware.
* Operating System. The NCS2 supports all of the same operating systems as OpenVINO, including Ubuntu, Windows 10, and MacOS.
* Precision. The NCS2 only supports FP16 model precision.
* Interface. The NCS2 has a convenient USB3.1 plug and play interface. Note that the NCS2 can be used on systems with only a USB2 port, but the inference will run slower due to I/O throttling.
* Cost. Compared to other AI accelerators, the NCS2 is an inexpensive option, typically costing around $70 to $100.
* Scalability. Adding multiple NCS2s (or other Myriad X devices) will allow multiple inferences to run in parallel.
* Size. All of these features come in a small size of 72.5mm X 27mm X 14mm, with the looks of a standard thumb drive.

### FPS vs. Power Tradeoff
One other characteristic that is important to note about the NCS2 is that it is meant to be a low-power device so that it can be easily deployed at the edge; however, one drawback of this is that it cannot process as many frames per second (FPS) as some other devices and thus it has a higher inference time.

For example, if we compare the NCS2 with an Atom E3950 processor, we can see that the Atom will have better FPS, but higher power requirements. The Atom processor (even though it is a relatively low-power processor) has about 12 times the power requirements of the NCS2. Thus, there is a tradeoff between power requirements and performance; the NCS2 is extremely low power, but this can come at some cost to performance as compared to the Atom.

### Exercise: VPU Scenario
West Indian Bank has a surveillance system in each of their branches. They are exploring AI-based computer vision capabilities for two of their branches at Nagpur and Thane.

**The Nagpur Branch**
In the Nagpur branch, they have an existing surveillance system that is currently monitored manually. The bank wants to enhance this system with edge AI to automatically detect potentially threatening individuals entering the bank.

The Nagpur branch has already invested a lot of money on its surveillance system, so it has a limited budget of only around $100 for new devices and would also like to keep power requirements as low as possible.

Provided below are the specifications for the Neural Compute Stick 2 and Xeon Processor.

Which of these would potentially work for the Nagpur branch?

ANs:
Power consumed
cost of the device

**The Thane Branch**
The Thane branch, in contrast, currently has an old, highly outdated surveillance system that they want to completely remove and replace. Since profits at this branch have been very high recently, the bank has a large budget to install a totally new system with up-to-date equipment. As part of this upgrade, they also want to install edge AI to detect suspicious or fraudulent behavior inside the bank.

As part of this new system, the bank will be installing about two dozen cameras that will all feed their data back to the edge device simultaneously. In order to effectively catch problematic behavior in time, inference on this data needs to be completed very quickly.

Which of these would potentially work for the Nagpur branch?
Xeon

### Multi-Device Plugin

not outdoor devices
form factors:
Seperrate devices: 
used for next unused devices,
same as plugin
PCIe express cards
HDDL

Reasons:
* load sharing techniique
* increase throughput
* Inference request accross varioous devices
* * MULTI : MYRAID CPU GPU
* Granular options 
* * MULTI :  CPU(2) GPU(2)

Applications 
* Need to saturate devices to see benefits
*  spreads inference request across all devices
* Reorder priority list

The order in which the devices are listed does prioritize the assignment of requests—so all of these commands will work, they would simply be prioritizing the devices differently.


This lesson is was all about Vision Processing Units (VPUs). In this lesson, we learned about:

The Architecture of VPUs
The characteristics of a particular VPU, the Myriad-X processor
The Neural Compute Stick 2 (NCS2), which uses the Myriad-X Processor
The characteristics of the NCS2
How to access the NCS2 on the DevCloud
OpenVINO's Multi-Device Plugin
How to run the same model on multiple devices at once using the Multi-Device Plugin
How to use the Multi-Device Plugin on the DevCloud


## FPGA's

In this lesson, we will learn about Field Programmable Gate Arrays (FPGAs). In the first half of the lesson, we'll look at the key concepts and characteristics that you'll need to know about FPGAs, including:

* Architecture of FPGAs
* Programming FPGAs
* Turning Your FPGA into an AI Accelerator
* Specifications of FPGAs
* Then we'll get some hands-on practice by:

Running inference using an FPGA in the DevCloud
Using the heterogeneous (HETERO) plugin


## Introduction to FPGAs

Before we talk about FPGAs, it helps to first consider an alternative: ASICs.

`Application-Specific Integrated Circuits (ASICs) are chips that are hardwired during manufacturing in order to be optimally efficient for a specific need.`

ASICs are often used in devices that have specific functions that aren't going to change over time. For example, you might want to create a circuit specifically for the backup camera in a line of cars. Developing this custom circuit is going to be very expensive, but the cost is offset by having an optimal circuit that can be used in a very large number of devices.

This is very different from an FPGA.

`Field-Programmable Gate Arrays (FPGAs) are chips designed with maximum flexibility, so that they can be reprogrammed as needed in the field (i.e., after manufacturing and deployment).`

This reprogrammability makes FPGA good for prototyping and low-volume production.


## Architecture of FPGAs

In the video, we looked at one small component of an FPGA: A tile or Adaptive Logic Module (ALM). A tile consists of three major blocks:

1. Configurable Logic Blocks (CLBs) form the core of the FPGA and there are typically thousands of these per FPGA. Each block can implement its own function using look up tables. These functions, for example, could be Boolean Operations like AND, OR and NOT. The logic blocks also contain flip flops, transistor pairs, and multiplexers.

2. Programmable Interconnects, which are made up of Connection Blocks (CBs) and Switch Block (SBs), steer the input and outputs of the CLBs. Notice how each Configurable Logic Block is interconnected in four directions—and we achieve the ability to program the logic through the ability to switch these connections on and off.

3. Programmable I/O Blocks connect the tile to an external circuit for input and output. These external circuits are external to the current tile, but still internal to the overall FPGA. They can be other tiles, Digital Signal Processing blocks (DSPs), memory blocks, or even more I/O blocks.

Again, this is only a small subset of an FPGA, which we are showing you so that you can get a general idea of why an FPGA is reprogrammable. For this course, it is not essential that you understand or remember all of the details of these components—the main point to take away from all this is simply that the FPGA's architecture makes it customizable. By turning the right connections on or off, we can essentially set up any logic configuration we like.

## Programming FPGAs

We've seen now that a key characteristic of FPGAs is that they are field-programmable. On the last page, we got a general idea of how the hardware's architecture makes this possible. But how do we actually program the FPGA's circuitry in practice? It turns out that there are a number of different ways we can approach this, at different levels of abstraction. Let's have a look.

Developing an program
HLL and LLL
Abstraction level (from above)
Register Transfer Level/Language(RTL)
Bitstream


Again, notice that the three lowest levels of abstraction all ultimately generate Register Transfer Level (RTL) output, which is then used to create a bitstream (and it is this bitstream that is fed into the FPGA to do the actual configuration of the logic blocks and interconnects). In contrast, OpenVINO leverages bitstreams that are pre-compiled and ready to be loaded at runtime.

## Converting an FPGA into an AI Accelerator
One of the advantages of an ASIC is that it is designed to carry out a particular type of task with optimal performance. The downside is that the ASIC is then optimized for that particular type of task, but does not perform as well (if at all) on other tasks.

In contrast, FPGAs can be reprogrammed to optimize their performance for different functions as needed. Thus, we can program an FPGA to act as an AI accelerator so that it performs well when running inference. However, we don't want to program it as a general AI accelerator—rather, we want to program it as an accelerator for the particular model we are going to run. Let's get an idea of what that might look like in practice.

## FPGA Specifications
In this section, we'll look at some of the specifications of FPGAs that differentiate them from other hardware types.


* High performance, low latency. Once programmed with a suitable bitstream, FPGAs can execute neural networks with high performance and very little latency. The high performance comes from the ability to run many sections of the FPGA in parallel. FPGAs also flow the data from one layer to the next, while keeping the data from one output to the next input layer on the same FPGA device. When running a neural network, we run the whole thing on the FPGA so the FPGAs don't go off-chip for the memory. This is faster than sending the output back to the CPU over the PCIe bus.

* Flexibility. FPGAs are flexible in a few different ways:

* * They are field-programmable; they can be reprogrammed to adapt to new, evolving, and custom networks
* * Various precision options (FP16, 11 and 9 bit ) are supported—allowing developers a balance between speed and accuracy.
* * The bitstreams being used can be updated without changing the hardware. This allows you to improve the performance of your system without replacing the FPGA.
* Large Networks. One feature of FPGAs that makes them especially useful in deep learning is that they can support large networks, with a capacity to handle networks that have more than 2 million parameters.

* Robust. FPGAs are designed to have 100% on-time performance, meaning they can be continuously running 24 hours a day, 7 days a week, 365 days a year. They are also able to function over a wide range of temperatures, from 0° C to 60° C. This means that FPGAs can be deployed in harsh environments like factory floors and still perform optimally.

* Long Lifespan. FPGAs have a long lifespan. For example, FPGAs that use devices from Intel’s Internet of Things Group have a guaranteed availability of 10 years, from start of production.


## Intel Vision Accelerator Design
Now that we know some of the key characteristics of FPGAs, we'll start looking at how to run inference on one using the Intel DevCloud. When you want to use an FPGA with OpenVINO on the DevCloud, you can use the Intel Vision Accelerator Design (VAD) with Intel Arria FPGA. We'll give this a try shortly, but first there are some things you need to be aware of to get OpenVINO to function as expected.

* The FPGA toolkit is only available for Linux distributions. You can develop a Python program in Jupyter Notebook on MAC or Windows and run it on the CPU, but when its deployed on an FPGA on the DevCloud the Edge System OS is Ubuntu. This means that filenames, paths, and devices—such as cameras—need to be converted to work on Linux. Most often the biggest change is the path to the network XML file, and the image or video source location.

* The network that you choose for your inference must have a matching bitstream loaded before inference. This matches up the primitive in the bitstream with the layers in the network. Remember, you're programming the FPGA to be a custom accelerator specifically for the network you want to run.

* The Deep Learning Acceleration (or DLA) can be used to add extensions to support custom layers. For this class, we will just be using the supplied bitstreams, but this is something to be aware of for later.

* The bitstream filename you're using in the DevCloud must be up to date. The bitstreams supplied with OpenVINO are specific to a particular OpenVINO version. Therefore, if you try an OpenVino example that was written for an earlier version of OpenVINO, you will need to update or edit the bitstream filename for the current version that is supported by the FPGA in the DevCloud.


### Exercise: FPGA Scenario
Mr. Ben works for a small pharmaceutical company that has a manufacturing factory in Ireland. It is important to analyze each drug package they manufacture before it is shipped off to customers. This analysis involves counting the number of drug bottles inside a package. They have a strict time period of 500 ms to analyze each package. If they fail to perform the analysis in this time frame, their whole manufacturing pipeline will get delayed.

The company is looking for ways to grow, but is much smaller than its competitors and has a very limited budget. Mr. Ben wants to find a low-cost way to ramp up production by 15% this quarter. To accomplish this, the analysis time will need to be improved so that it is completed within 300ms.

At the moment, the company is using CPUs to perform this analysis, but the current processors are already functioning at capacity in order to achieve the 500 ms analysis time.

Which of the following requirements are essential in considering the hardware for this manufacturing factory?
Ans:
ONly the cost

Which hardware would be most appropriate for the manufacturing plant?
Ans:
NCS2

With the increase in production made by reducing the analysis time, Mr. Ben was able to significantly improve revenue this quarter. He now wants to ramp up his production further and has set a new target of completing the analysis within the much shorter time window of 50 ms.

Furthermore, Mr. Ben wants to make his factory lean and adapt to changing market requirements. So, instead of manufacturing just drugs, he now also wants to manufacture other pharmaceutical products and possibly even medical equipment.

Thus, Mr. Ben would like to install a new system that not only dramatically improves his inference time, but can also be modified as needed to do an analysis of other products.

Which of the following factors are relevant in this case?
ANs:
Flexibility/Performance/ BUdget

job_id_core = !qsub load_fpga_model_job.sh -d . -l nodes=1:tank-870:i5-6500te:iei-mustang-f100-a10 -F "HETERO:FPGA,CPU /data/models/intel/vehicle-license-plate-detection-barrier-0106/FP16/vehicle-license-plate-detection-barrier-0106"
print(job_id_core[0])


As you can see, FPGAs have limited layer support. And attempting to run an unsupported layer will cause the app to crash. What we would like to do is be able to run our model primarily on the FPGA, but if an unsupported layer is needed, have this run on a device where it is supported rather than crashing. We can accomplish this goal using OpenVINO's heterogeneous (HETERO) plugin.

The heterogeneous (HETERO) plugin allows you to specify the primary device, as well as one or more fallback devices that should be used in the event that the primary device does not support the layers in your model.

The HETERO plugin enables efficient hardware utilization by running inference for one model across several hardware devices.


## Scenario 1: Manufacturing Sector
Below is the first scenario. Your job is to read through the scenario and figure out which type of hardware might best fit the client's needs. You'll see that this is similar to scenarios you practiced with earlier in the course.

The scenario is pretty long and not all of the information is important! That's intentional and not unlike what you'll encounter when working with real clients. This is an opportunity to demonstrate that you know which pieces of information are relevant when selecting hardware, and which are unimportant or unrelated.

The Scenario
Mr. Vishwas is the VP of Engineering at Naomi Semiconductors, a manufacturer known for its industrial-grade standard in producing semiconductor chips. Recently, the company has been venturing into Intel Pentium 4/3000 chip production—and they want to maximize their revenue in this venture. Their other chips in the last year have earned them two million dollars alone. With such good revenue, their expansion into the Intel Pentium 4/3000 industry is an obvious next step.
`cost to spare`

There are several steps involved in the chip manufacturing process:

Step 1: Produce a silicon ingot
Step 2: Create blank wafers
Step 3: Use these wafers to reproduce a patterned wafer
Step 4: Create and test dies
Step 5: Assemble bond dies into packages
Step 6: Test packaged dies
Step 7: Ship dies to customers
Mr. Vishwas explains that there have been several roadblocks in this pipeline. The entire process should take around 6 to 8 weeks—but currently, it is taking 10 to 12 weeks. This is reducing their revenue by 30%.

`need new upgrade to reduce the time`

Mr. Vishwas has noticed that Step 7 (shipping to customers) seems to be taking the most time. This part of the process involves the manual labor of packaging the chips into boxes. There is one particular shop floor—which has two industrial belts—that has shown slower production than the rest.

Workers alternate shifts to keep the floor running 24 hours a day so that packaging continues nonstop, but Mr. Vishwas has noticed a slow-down in production during the shift transition periods. Between shifts, he has observed a 70% dip in the production rate of packaged containers.

To help understand and address these issues, Mr. Vishwas wants a system to monitor the number of people in the factory line. The factory has a vision camera installed at every belt. Each camera records video at `30-35 FPS (Frames Per Second)` and this video stream can be used to monitor the number of people in the factory line. Mr. Vishwas would like the image processing task to be completed `five times per second.`

Once this productivity problem has been addressed, Mr. Vishwas would like to be able to repurpose the system to address a second issue. The second issue Mr. Vishwas has encountered is that a significant percentage of the semiconductor chips being packaged for shipping have flaws. These are not detected until the chips are used by clients. If these flaws could be detected prior to packaging, this would save money and improve the company's reputation.

To be able to detect chip flaws without slowing down the packaging process, `the system would need to be able to run inference on the video stream very quickly.` Additionally, because there are multiple chip designs—and new designs are `created regularly`—the system would also need to be `flexible so that it can be reprogrammed and optimized` to quickly detect flaws in different chip designs.

`fpga to be reprogrammed`

While Naomi Semiconductors has plenty of revenue to install a quality system, this is still a significant investment and they would ideally like it to last for at least 5-10 years.

`best of teh among is fpga`


## Scenario 2: Retail Sector

Below is the second scenario. Again, your job is to read through the scenario and figure out which type of hardware might best fit the client's needs.

The Scenario
PriceRight Singapore has one of its smaller outlets in the tiny neighborhood of Dover. Mr. Lin is the store manager—and like any good store manager, `he wants to use Edge AI to help maximize` his profit this year.

Most of the customers are regulars at the store. Mr. Lin has seen an average of about `200 people in the store during weekdays`. On the `weekends, this increases to between 500 and 1000`. The `maximum number of people visit the store during the holidays`. Most customers spend `30-50 mins in the store during a single visit`. Out of this, they have an `average wait time of 230 seconds at the checkout counters`. But on the weekends, the wait time can increase substantially. `The average time spent is 40 mins at the store and 350-400 seconds at the checkout line`.

The total number of people in the checkout queue ranges from an `average of 2 per queue (during normal daily hours) to 5 per queue (during rush hours)`.

It is during rush hours that Mr. Lin has seen wavering sales. `When wait times are short` and checkout happens smoothly, he sees a jump in his `revenue from 6 to 20%`. However, if there is congestion at the `checkout counter, his profits only go up to 4-5%`.

Mr. Lin believes this problem can be easily solved by directing people to less-congested queues in the store, and he is interested in using an Edge AI system to do so.

Most of the store's checkout counters already have a modern computer, each of which has an `Intel i7 core processor`. Currently these processors are only used to carry out some minimal tasks that are not computationally expensive.

Mr. Lin employs close to `300 employees,` including staff that work in transportation, on the store floor, and at the checkout counter. Although the `store's annual sales are $7 million in food alone`, the net profit is only `about 1.1% of this`. Mr. Lin also believes in giving fair employment and good wages. He pays his staff with proper salaries, along with substantial `bonuses twice a year`. As a result, `Mr. Lin does not have much money to invest in additional hardware`, and also would like to save as much` as possible on his electric bill`.


## Scenario 3: Transportation

Below is the third scenario. Again, your job is to read through the scenario and figure out which type of hardware might best fit the client's needs.

The Scenario
Ms. Leah is the Innovation head for Delhi Metro Rail Services. Delhi Metro is an urban passenger transportation system connecting Ghaziabad, Faridabad, Gurgaon, Noida, Bahadurgarh, and Ballabhgarh in the National Capital Region of India. `Delhi Metro makes 2,700 trips every day and is one of the busiest metros in India`.

During peak hours, some areas of the platform get `highly congested`, while other areas remain relatively open. In some cases, passengers trying to board in the more congested areas are unable to get on, even though there is space on the train.

Currently, this congestion is handled manually by door operators, who help direct passengers to less congested areas during peak time. Ms. Leah would like to automate this using an Edge AI system that would monitor the queues in real-time and quickly direct the crowd in the right manner.

In peak hours `they currently have over 15 people on average in a single queue outside every door `in the Metro Rail. But during non-peak hours, the number of people `reduces to 7 people in a single queue.` On office hours there is a train every 2 mins. However, on the weekends the `time increases to up to 5 mins since some of their drivers work only 5 days a week.`

`They monitor the entire situation with 7 CCTV cameras on the platform`. These are connected to closed All-In-One PCs that are located in a nearby security booth. `The CPUs in these machines are currently being used to process and view CCTV footage for security purposes and no significant additional processing power is available to run inference`. Ms. Leah's budget allows for a maximum of `$300 per machine`, and `she would like to save as much as possible both on hardware and future power requirements`.
