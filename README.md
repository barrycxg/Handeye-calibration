# Handeye-calibration
Handeye calibration using ICP method in UR robot. 

This document contains two camera calibration methods: eye in hand and eye to hand. And the test files of extrinsic parameters calibration are also covered. 

## Installation
This implementation requires the following dependencies (tested on Ubuntu 16.04 LTS, Ubuntu 18.04 LTS and Ubuntu 20.04 LTS):
* You can quickly install/update these dependencies by running the following:

    ```shell
    python setup.py install
    ```

* If you need to use realsense camera, you can download from the official library([RealSense](https://github.com/IntelRealSense/librealsense)).

* The UR robot control package version is recommended to use 1.4.4,1.4.7, 1.4.8, 1.4.9.

## Calibrating Camera Extrinsics
We provide a simple calibration script by using the checkerboard to estimate camera extrinsics with respect to robot base coordinates and gripper coordinates.

We add automatic calculation of initial trans files. In actual operation, we use the aruco marker, determine the X, Y, and Z directions of the aruco marker coordinate system in advance, align it with the robot base coordinates, and manually adjust the robot end point to align with the center of the aruco marker to obtain the initial calibration matrix.

### Eye to hand calibration

* If you need to determine the transformation matrix between the camera coordinate system and the robot base coordinate system，you can run the following command:

    ```shell
    python get_init_trans_eyetohand.py
    python calibr_eye_to_hand.py [--use_recorded_data]
    ```

`[--use_recorded_data]` indicates the code only use the previous calibration data for calibration.

### Eye in hand calibration
Before calibration, we need to drag the robot to determine the position of calibration board in the base coordinate system. Then we collect the calibration data for obtaining the extrinsic of camera. 

* The whole process can be realized by executing the following command:

    ```shell
    python get_init_trans_eyeinhand.py
    python calibr_eye_in_hand.py [--use_recorded_data]
    ```
`[--use_recorded_data]` indicates the code only use the previous calibration data for calibration.

## Test Calibration Results
We use the aruco board and opencv for getting the pose of the aruco board and we make the robot grasp the board. The following command:

* Eye to hand

    ```shell
    python calibr_test_eyetohand.py
    ```

* Eye in hand

    ```shell
    python calibr_test_eyeinhand.py
    ```

### Note

When you modify the size of aruco, you need to change the second parameter in calibr_test_eyetohand.py and calibr_test_eyeinhand.py

    rvec, tvec, _ = aruco.estimatePoseSingleMarkers(corners, 0.06, K, dist)
