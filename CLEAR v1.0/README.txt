The main way to use it is log.logstart(). But first we need to import the file.
The main way to do it is to in a function (name does not matter) put an "if brain.sdcard.is_inserted():" as the first thing then a try: and the error handling for the import error and a name error then in the try add import CLEAR then Clear= CLEAR to get the file on a variable then do the log stuff you want the recommended way is the log.logstart() function but you will need to write it as Clear.log.logstart() because of it being imported. For the inputs for the function, there are only two inputs required: the right and then the left drivetrain motors. If you have more than 2 motor drivetrains, add them in a 123 right-left pattern, like "right1 left1 right2 left2". Then, if you want any other motors in it, add them by adding motor1=, motor2=, etc., to the motor(s) you want to log. The next input is the controller; it can specify both controllers by saying controller1=, then the name of the main controller, and then controller2= for the secondary controller. If you want variables, use log.add_logstart() and put them in quotation marks; add log.capture.variable() then add the name as a string and the value by putting in just the variable. Then, out of the function, add Thead() and put the function in it without the parentheses. At the end, it should look like this:

if brain.sdcard.is_inserted() and brain.sdcard.exists("CLEAR.py"):
     import CLEAR

     def capture_setup():
     	 CLEAR.log.add_logstart("log.capture.variable('variable', variable)")
         CLEAR.log.logstart(Right1, left1, Right2, left2, Right3, left3, motor1=enter up to six motors)
     # NOTE: for the stuff that can take up to six things, it should go like motor1=x motor2=y, etc. NOT: motor1=(x, y, etc.)
     # NOTE2: There are a few extra things you can configure to change if you want to print to the brain screen or not index the log history. Syntax: brainscreen=True, indexhistory=False.
     Thread(capture_setup)

All of that can use logging, capture, and archive. For the code to use recording and encoding, we need another function in the main code. There are two ways to do this: record and then encode, or record and encode on two separate inputs. We recommend the first option. Please create a function (name doesn't matter) with the same if and error handling, and don't import it. You'll need the previous code to enable recording and encoding. Add the CLEAR.log.recording.start() then add the recording name to the input. After you've finished the recording, add CLEAR.log.record.stop() and also add the recording name (you may add the stop as a toggle or as a separate function). After you stop the recording, please put in CLEAR.log.recording.encode() now in the inputs you add the right side drive function, then the left, then any function needed from the recording as its function, then its stop function, then its button (the max you can add is six), and it should look like this with some controller print to show if it is recording and when it's done:

def record():
            global recording_state
            if recording_state == 0:
                CLEAR.log.recording.start("enter name of file you want here")
                controller_1.screen.clear_line(3)
                controller_1.screen.set_cursor(3,1)
                controller_1.screen.print("recording.")
                recording_state=1
            elif recording_state == 1:
                CLEAR.log.recording.stop("Right")
                controller_1.screen.clear_line(3)
                controller_1.screen.set_cursor(3,1)
                controller_1.screen.print("Stopped.")
                CLEAR.log.recording.encode("enter name of file you want here", enter function for right side drive, enter function for left side drive, "enter other functions up to six here", enter start of function, enter stop for function here if there is no stop enter none, "enter button you want for other function")
                controller_1.screen.clear_line(3)
                controller_1.screen.set_cursor(3,1)
                controller_1.screen.print("Encoded.")
                recording_state=0

 #callback to run the function if the button is pressed
        controller_1.buttonRight.pressed(record)
 
 # end of recording part of code

     controller_1.buttonUp.pressed(recallhistory)

     def aton():
         CLEAR.log.recording.run("Right")

     def driver():
         pass
     #Enter driver-based functions you want here
        
     comp=Competition(driver, aton)

However if you do not feal confident doing this or just do not understand it you may also use the log.auto_start() this is a one time function call that auto configures the CLEAR code to work with most codes. you can simply copy the text bellow and put it under the configuration of your code.

# Code to configure your robot / variables declarations here.
 if brain.sdcard.is_inserted() and brain.sdcard.exists("CLEAR.py"):
    import CLEAR
    logging=Thread(lambda: CLEAR.log.auto_start(True))
# Other code.
