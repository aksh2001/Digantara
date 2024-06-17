# Satellite Position Processor

This project processes satellite position data using TLE (Two-Line Element) sets and calculates their positions at one-minute intervals over a day. It then filters these positions based on specified regional coordinates. Built using python and jupyter-notebooks.

## Technologies Used:
- [Python](https://www.python.org/) - High-level, general-purpose programming language.
- [Jupyter-Notebook](https://jupyter.org/) - Notebook document for the project.
- [Numpy](https://numpy.org/) - For Efficient data structures to store mathematical data.
- [SGP4](https://pypi.org/project/sgp4/) - To get satellite location from TLE.
- [pyproj](https://pypi.org/project/pyproj/) - To convert into satellite location to latitude, longitude and altitude data 
- [DASK](https://www.dask.org/) - Python Library for Parallel Computing.

## How to Use:
```
   >>https://github.com/aksh2001/Digantara.git
   >>cd Assignment_Code Optimization
   >>cd Assignment_Code Optimization
   >>jupyter notebook "Abhinav-Choudhary_202A8TS0573P.ipynb"
```
## Steps:

### Step 1: Get Satellite Location:
This library contains functions to retrieve the location of a satellite using TLE data attached in the text document.
Retrieve the position of each satellite for one day using a one-minute time interval.
Obtain the data in the following format: time, L(x), L(y), L(z), V(x), V(y), V(z), where L
represents location and V represents velocity
### Step 2: Convert Satellite Location:
Convert data to lat long alt format using pyproj library
```
def ecef2lla(i, pos_x, pos_y, pos_z):
    ecef = pyproj.Proj(proj="geocent", ellps="WGS84", datum="WGS84")
    lla = pyproj.Proj(proj="latlong", ellps="WGS84", datum="WGS84")
    lona, lata, alta = pyproj.transform(ecef, lla, pos_x[i], pos_y[i], pos_z[i], radians=False)
    return lona, lata, alta
```
### Step 3: Results:
Ask the user for the coordinates and compare with the processed data of each satellite and count the number of satellite appearing within the coordiantes.
This program is scaled to ingest about 30000 satellites [sample file -30000sats.txt]
The optimized version uses Dask to parallelize the processing of satellite data. Dask helps in parallel computing by dividing the work into smaller tasks and distributing them across multiple CPU cores or even across a cluster of machines. This can significantly speed up the processing time for large datasets.
```
    client = Client(n_workers=16, threads_per_worker=1, memory_limit='0.5GB')  # This will start a local Dask scheduler and worker
    print(client)
```
<Client: 'tcp://127.0.0.1:57089' processes=16 threads=16, memory=7.45 GiB> - The Client created for this task. We use 16 cores and 16 threads, and give each process about 0.5 GB of memeory limit. The program takes 32.6 s to calculate the results which an huge improvement compared to non optimized version which can takes several minutes or hours to compute.
- Created by Abhinav Choudhary, as part of hiring assignment of Digantara