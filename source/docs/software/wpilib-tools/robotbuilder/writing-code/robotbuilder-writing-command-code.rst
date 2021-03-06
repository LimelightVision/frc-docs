Writing the Code for a Command
==============================

Subsystem classes get the mechanisms on your robot moving, but to get it to stop at the right time and sequence through more complex operations you write Commands. Previously in :doc:`writing the code for a subsystem <robotbuilder-writing-subsystem-code>` we developed the code for the Claw subsystem on a robot to start the claw opening, closing, or to stop moving. Now we will write the code for a command that will actually run the Claw motor for the right time to get the claw to open and close. Our claw example is a very simple mechanism where we run the motor for 1 second to open it or 0.9 seconds to close it.

Closeclaw Command in RobotBuilder
----------------------------------

.. image:: images/writing-command-code-1.png

This is the definition of the Closeclaw command in RobotBuilder. Notice that it requires the Claw subsystem. This is explained in the next step.

Generated Closeclaw Class
-------------------------

.. tabs::

   .. code-tab:: java

      public class Closeclaw() {
         // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
      }

      // Called just before this Command runs the first time
      protected void initialize() {
         setTimeout(1.0);
         Robot.claw.closeClaw();
      }

      // Called repeatedly when this Command is scheduled to run
      protected boolean execute() {

      }

      protected boolean isFinished() {
         return isTimedOut();
      }

      // Called once after isFinished returns true
      protected void end() {
         Robot.claw.stop();
      }

      // Called when another command which requires one or more
      // subsystems is scheduled to run
      protected void interrupted() {
         end();
      }

   .. code-tab:: cpp

      #include "Closeclaw.h"

      Closeclaw::Closeclaw() {
         // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         Requires(Robot::claw);
         // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         SetTime(1);
      }

      void Closeclaw::Initialize() {
         Robot::claw->Close();
      }

      void Closeclaw::Execute() {

      }

      bool Closeclaw::IsFinished() {
         return IsTimedOut();
      }

      void Closeclaw::End() {
         Robot::claw->Stop();
      }

      void Closeclaw::Interrupted() {
         End();
      }

RobotBuilder will generate the class files for the Closeclaw command. The command represents the behavior of the claw, that is the operation over time. To operate this very simple claw mechanism the motor needs to operate for 1 second in the close direction. The Claw subsystem has methods to start the motor running in the right direction and to stop it. The commands responsibility is to run the motor for the correct time. The lines of code that are shown in the boxes are added to add this behavior.

1. Set the one second timeout for this command. When the command is scheduled, a timer will be started so the one second operation can easily be tested.
2. Start the claw motor moving in the closing direction by calling the Close method that was added to the Claw subsystem.
3. This command is finished when the timer runs out which happens after one second has passed. This is the timer set in step 1.
4. The ``End()`` method is called when the command is finished and is a place to clean up. In this case, the motor is stopped since the time has run out.
5. The ``Interrupted()`` method is called is this command is interrupted if another command that also requires the Claw subsystem is scheduled before this finishes. For example, if the Closeclaw command was scheduled and running, then the Openclaw command was scheduled it would interrupt the Openclaw command, call its Interrupted() method, and the motor would stop.
