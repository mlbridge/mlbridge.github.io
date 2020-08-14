<p float="left" align = "center">
  <img src="readme-assets/ML_Bridge_Logo.png" width="600"/>
</p>

# Overview

This organisation is the result of my work on a project during Google Summer of 
Code 2020, as a part of the Cloud Native Computing Foundation.

The ML Bridge organisation provides machine learning capabilities to languages 
and platforms that generally have a dearth of such capabilities. 

Currently, it is being used to integrate machine learning capabilities with 
CoreDNS, to protect people against malicious websites and applications. It helps
in identifying websites that could be potentially used by malicious hackers and 
cybercriminals and prevents the user from accessing such websites. 

# Approach

## General Overview

CoreDNS is a DNS Server written in Go. However, Go currently does not have 
native libraries for the interaction with the CUDA platform, which is essential 
for machine learning applications. At the same time, the Python ecosystem has 
tools like TensorFlow, PyTorch, MXNet and various others that not only interact 
with the CUDA platform but also allows for the easy prototyping and evaluation 
of deep learning models.











