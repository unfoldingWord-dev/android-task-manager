#Task Manager
A library for sanely performing threaded tasks that are accessible from the UI thread.

The task manager provides a way for an application to perform certain tasks that need to be kept
track of without breaking the ui. When you queue a task you receive a task id (or you can provide your own) that can be used
to check up on the task at a later time, in a different thread, or in a different activity.

##Installation
To use this library your Android project must be configured to use the JCenter or Maven Central repositories.

Add the following to your package dependencies and sync gradle.
```
compile 'org.unfoldingword.tools:task-manager:1.5.0'
```

##Example Usage
Here is a popular way to use the task manager.
This example does not cover storing the task id for later reference.
You could use `onSavedInstanceState` or some other custom method for keeping track of the id.

```
public int connectToTask(String taskId) {
    MyManagedTask task = (MyManagedTask) TaskManager.getTask(taskId);
    if(task != null) {
        // connect to existing task
        task.setOnFinishedListener(this);
        task.setOnProgressListener(this);
    } else {
        // start new task
        task = new MyManagedTask();
        task.setOnFinishedListener(this); // assuming this class implements the listener
        task.setOnProgressListener(this); // assuming this class implements the listener
        taskId = TaskManager.addTask(task);
    }
    return taskId;
}
```

##Creating Tasks
You can create your own tasks by excenting the `ManagedTask` class. Or you can create anonymous tasks directly

```
ManagedTask task = new ManagedTask() {
    @Override
    public void start() {
        //... some code
    }
};
TaskManager.addTask(task);
```