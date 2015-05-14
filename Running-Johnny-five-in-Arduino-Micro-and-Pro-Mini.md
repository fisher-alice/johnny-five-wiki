If you have an problem when running Johnny-five with this boards, and you get an Device or firmware error its maybe a Board/Firmata misconfiguration.

To solve this before upload Firmata, search the following `Firmata.begin` and replace the 57600 value for 28800, then upload the sketch and try to run Johnny-five and you shouldn't see any error message.