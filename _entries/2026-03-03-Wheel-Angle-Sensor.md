---
layout: entry
date: 2026-03-03
clusters:
  - vehicle-dynamics
    - electronics
    excerpt: |
      Magnetic encoders can be used to measure a whole lot of things
      status: in progress...

      ---

      # Bolt securing inner bearing of RodEnd or Slip Bearing does not rotate with the rest of the rod-end housing #
      <video style="width: 100%; max-width: 600px; height: auto;" src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/BoltFixedWRTWheel.mp4" controls></video>

      ---

      As the video above demonstrates, when observing the bolts securing the Tie-rod rodends and the upper upright eyelet, we can see that the bolt remains fixed in angular position with respect to the wheel. Hence, measuring the bolt orientation with respect to the chassis will effectively allow us to measure the wheel heading with respect to the chassis (Wheel Angle).

      ---

      # Inner wheel lock and Outer wheel lock conditions with relative angle between bolt head angular position and Tie-rod #
      <img src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/SteeringKnuckleFullExtensionMarkedUp.JPG" alt="Steering Knuckle Extension" style="width: 100%; max-width: 600px; height: auto;">

      <img src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/SteeringKnuckleFullRetractionMarkedUp.JPG" alt="Steering Knuckle Retraction" style="width: 100%; max-width: 600px; height: auto;">


      θᵢ = Inner Wheel Lock angle and θₒ = Outer Wheel Lock Angle
      To draw the reference axes, the longitudinal axis of the Tie-rod and two corners of the Hexagon on the Allen bolt head were chosen (The same two corners). This helps us visualise the relative angular displacement that we are trying to measure.

      The range of angles can be seen in the following videos:

      <video style="width: 100%; max-width: 600px; height: auto;" src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/RotationAboutSteeringKnuckles1.mp4" controls></video>
      <video style="width: 100%; max-width: 600px; height: auto;" src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/RotationAboutSteeringKnuckles2.mp4" controls></video>

      ---

      # The above analysis can be similarly done at the Upright Upper A-arm eyelet: #
      <img src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/UpperUprightBoltInnerLock.jpeg" alt="Upper Upright Bolt Inner Lock" style="width: 100%; max-width: 600px; height: auto;">

      <img src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/UpperUprightBoltOuterLock.jpeg" alt="Upper Upright Bolt Outer Lock" style="width: 100%; max-width: 600px; height: auto;">

      <video style="width: 100%; max-width: 600px; height: auto;" src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/UpperUprightBoltRotation.mp4" controls></video>

      ---

      # Implementing the Magnetic Encoder #
      The magnetic encoder measures the angular position of a diametrically magnetised neodymium magnet. Attaching this magnet to the bolt head will now couple the wheel heading to the magnet's angular position.
      A special fixture will have to be made to attach the magnetic encoder to the Tie-Rod or the Eyelet plate, such that the axis of the bolt will coincide with the position of the magnetic encoder IC. Refer the below image for a visualisation:

      <img src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/WheelAngle_Media/Implementation.jpeg" alt="Magnetic Encoder Implementation" style="width: 100%; max-width: 600px; height: auto;">

      The wheels will be brought to 0 degrees and the encoder values will be tared. The relative angle from this zero position can now be recorded.
      
