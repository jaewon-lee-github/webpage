# Note for evertyhing

## git
### How to fix that gihub ask me the password always.
  - git config --global credential.helper store
  - https://www.freecodecamp.org/news/how-to-fix-git-always-asking-for-user-credentials/
  
## Markdown
### add nesting indentation
  - add four spaces than previous line in front of the nesting numbers
 
## Pocl
### How newlib device target create thread on RISC-V
#### Generate Host+Device binary
  1. The newlib device target creates RISCV binary that includes both the host and kernel code merged together.
  1. Generate Kernel code during the compilation phase by calling pocl_llvm_build_newlib_program()(line 558).
  1. This call will create a kernel blob (static library) that includes  
      1. the kernel function body  
      1. the kernel registration function _pocl_register_kernel (line 579)  
      1. a static global object auto_register_kernel_t (line 616)
  1. To generate the final Newlib program, we merge the OpenCL host program with the Kernel blob described above to get a combined RISC-V binary.
#### Launch the binary 
  1. When you launch the binary, the C++ runtime will first initialize all global objects including auto_register_kernel_t
  1. we use this C++ facility to invoke auto_register_kernel_t 's constructor which internally will call _pocl_register_kernel() and pass the kernel arguments (line 608).
  1. After the C++ runtime initializes all global objects, it will call the main() entry point of the given host program, 
  1. Then somewhere down the road it will initialize the OpenCL context via clCreateContext()
  1. this call will eventually call pocl_newlib_init_device_ops(), 
  1. Call pocl_newlib_get_builtin_kernel_metadata()
  1. Call _pocl_query_kernel()
### References
  - Pocl Paper: https://arxiv.org/pdf/1611.07083.pdf
  - Casl Pocl: https://github.gatech.edu/casl/pocl/
  - Pocl internal: https://trepo.tuni.fi/bitstream/handle/123456789/22636/Korhonen.pdf?sequence=3&isAllowed=y
  
## HIP && HIPCL
### References
  - HIP Presentation: https://www.olcf.ornl.gov/wp-content/uploads/2019/10/Roth-HIP-on-Summit-20191009.pdf
  - HIPCL paper: https://dl.acm.org/doi/pdf/10.1145/3388333.3388641
  - HIPCL github(Release) https://github.com/cpc/hipcl/tree/release_0_9
  - HIPCL introduction: https://www.computer.org/publications/tech-news/from-cuda-to-opencl-execution

## SPIR-V
### References
  - http://llvm.org/devmtg/2017-03//assets/slides/spirv_infrastructure_and_its_place_in_the_llvm_ecosystem.pdf
  - https://www.khronos.org/blog/offline-compilation-of-opencl-kernels-into-spir-v-using-open-source-tooling

## Misc.
  - https://github.com/gthparch/cuda-on-everywhere
