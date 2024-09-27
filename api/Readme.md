## Dockerfile Overview
This API uses a custom Dockerfile to build the PHP environment for running a Laravel API application. Below is an explanation of the key components and techniques used in the Dockerfile:

### Base Image
The Dockerfile is based on the PHP 8.2 CLI image running on Debian Buster, This provides the foundation for running PHP scripts and includes a minimal operating system.

### System Dependencies
Includes install essential system dependencies required for PHP extensions and Composer, These dependencies include development libraries, MySQL client tools, and utilities for handling image manipulation, XML, and zip files.

### Image Optimization
To keep the image size small and efficient, i clean up unnecessary files after installing the dependencies.

### Multi-stage Installation
Installing dependencies and PHP extensions in a single run to avoid creating extra layers.

### Additional Cleanup
Unnecessary PHP and OPcache files are removed to reduce image bloat.

### Composer Installation
The Composer dependency manager is installed globally, allowing us to manage PHP dependencies within the container.

### Exposing Ports
Ensuring the container’s services can be accessed externally.
