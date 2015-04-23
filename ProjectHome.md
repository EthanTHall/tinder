Tinder follows MVCS pattern:

**M(model):** handles application's data in runtime.

**V(view):** UI components and screen elements for visualizing data and handles user interaction.

**C(controller & commands):** receive user interaction events, and assigns the events to appropriate command, then commands will optionally call appropriate Service Layer to send requests, get responds, then update model and view.

**S(service):** handles service URLs and actually send/get requests/responds in specific format (xml,e4x,string...)

**In Tinder:**

View(ui components) will bubbles user interaction events up to SystemManager(SystemManager as a root of Flex application will catch all events). that makes the view don't care about application business logic, just visualize data, receives user interaction and triggers events, like the concept of Flex build-in ui components.

Controller will map specific event to specific command, when the specific event is triggered then the related command will be instantiated and be executed.

Command will be instantiated only when the command will be executed, and will be destroyed after executing completed.
command handles the references of view, model and service.

**v is view,** command can receive ui components which with the id started with '$', so v.loginPanel is the LoginPanel instance with id "$loginPanel" and doesn't care about where LoginPanel is placed.

**m is model,** for example: m.user will receive the user data which is stored in model.

**s is service,** if the command implements IRespond interface, it can call service through s, then after the request got result, the "result" method will be invoked by framework, otherwise, the "fault" method will be invoked.

command can also call other commands by runCommand() method, that makes commands very easy to be reused.
command also has an error handle function, you can call "error" method to visualize current error to user, it will be an Alert poping up by default. you can setup a customed ErrorHandleClass which is also a command for visualizing error information.