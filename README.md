# QuadTelemetrySuite
Visualization suite running in a local web browser to visualize relevant state information of a quadrotor in flight.

---
# Front End Visualization


---
# IoT Access

## ESP32

An ESP32 chip will be added to the flight computer in order to send packets of data up to the front end in order to visualize the results. A custom arduino ESP32 library wrapper will be written to make the process of writing requests more streamlined. The ESP32 can make either TCP or UDP request to a host server. 

---
# Sensors

## IMU

The IMU will calculate the three euler angles, roll, pitch, and yaw, that represent the current attitude of the quadrotor. The coordinate system for the quadrotor will be calculated with respect to an ECEF coordinate frame. To do this it will use three seperate sensors. An accelerometer and gyroscope will have their readings combined via a complimentary filter in order to determine the pitch and roll of the quadrotor. The magnetometer will figure out the yaw of the quadrotor by sensing it's heading compared to due north. 

### Accelerometer

The accelerometer is onboard the MPU6050. It measures the acceleration of the quadrotor in the three cardinal directions. Using the direction and magnitude of the gravtiy vector the attitude of the quadrotor can be determined. However the accelerometer also measures any other accelerations on the quadrotor so the readings are usually very noisy. Averaging the readings will remove the noise and give a decent estimate of the true attitude but by combining the accelerometer readings with the gyroscope readings a better estimate can be acheived.

### Gyroscope

The gyroscope is onboard the MPU6050 as well. measures the angular rates of the quadrotor. By integrating the gyroscope angular rate readings and assuming they are constant over the sampling interval a decent estimate for the attitude of the quadrotor can be achieved as their readings are low noise. If the sampling rate is significantly small this method will be very accurate, but over time the estimate will drift from the true reading due to the fact that the rates are not constant over the sampling interval. By combining the accelerometer and gyroscope readings a better estimate can be acheived. 

### Complimentary Filter

A complimentary filter is used to combine the gyroscope and accelerometer readings to get a better estimate for the roll and pitch of the quadrotor. Essentially the filter is a weighted average between the gyroscope and the accelerometer readings designed to mitigate the sensor noise while slowly canceling out the gyroscope drift. The weights are 99.3 for the gyroscope readings and .7 for the accelerometer readings.

### Magnetometer 

SENSOR TO BE DETERMINED. The Magnetometer will measure the heading of the quadrotor with respect to north by sensing earth's magnetic field acting on the quadrotor. The quadrotor is using an ENU coordinate frame and so due north is 0 in terms of the yaw euler angle. 
