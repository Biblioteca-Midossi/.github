# Frontend documentation 📚
Frontend application for [Midossi](https://www.midossi.edu.it/)'s digital library.

## Installation
### Install Node
Download Node from the [official site](https://nodejs.org/en)

> [!Note]
> This project was developed using Node 20 and newer versions, and might not work with older versions

### Clone this repository
To clone the repository, use one of the following commands depending on whether the repository is public or private:

#### Public Repositiory:
```bash
git clone https://github.com/Biblioteca-Midossi/Frontend.git
```
#### Private Repository:
Use [GitHub Desktop](https://desktop.github.com/) or:
```bash 
git clone git@github.com:Biblioteca-Midossi/Frontend.git
```
> [!Note]
> Authentication with SSH is required for private repositories.

### Install dependencies
We recommend using [yarn](https://yarnpkg.com/) (Windows) or [Bun](https://bun.sh/) (Unix-like systems) as package managers.
Please make sure you install yarn using [Corepack](https://nodejs.org/api/corepack.html).

```bash 
# Yarn (Corepack)
yarn install

# Bun
bun install
```

### Configure the environment
Rename `.env_template` to `.env` and fill in the appropriate credentials:
```Shell
# This indicates the API location. localhost and 127.0.0.1 are known to not work. If you're using the same machine to host
# backend and frontend (like we do), use a private network address
VITE_HOST=192.168.172.10:8000
```

## Running the application
On Unix-like systems (Using Bun):
```bash
bunx --bun vite
```




