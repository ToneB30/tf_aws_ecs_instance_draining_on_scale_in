# ECS Instance Draining on Scale In
===================================

Heavily inspired by this AWS [blog post](https://aws.amazon.com/blogs/compute/how-to-automate-container-instance-draining-in-amazon-ecs/), this module deploys resources and code to support ECS Instance Draning and ASG lifecycle hook to ensure that running tasks are not obliterated by ASG scale-in events.

Further details about [AutoScaling Lifecyle Hooks](http://docs.aws.amazon.com/autoscaling/latest/userguide/lifecycle-hooks.html) is available.

![alt tag](https://s3.amazonaws.com/chrisb/Architecture.png)


Module Input Variables
----------------------

- `autoscaling_group_name` - The Name of the AutoScaling Group used by the ECS Cluster.
- `hook_heartbeat_timeout` - Amount of time, in seconds, the lifecycle hook should wait before giving up and moving onto the default result. Defaults to 900 (15 mins).
- `hook_default_result` - Can be one of either ABANDON or CONTINUE. ABANDON stops any remaining actions, such as other lifecycle hooks, while CONTINUE allows any other lifecycle hooks to complete. Default is ABANDON
- `enabled` - boolean expression. If false, the Lifecycle Hook is removed from the AutoScaling Group. Defaults to `true`.

Usage
-----

```js
resource "aws_autoscaling_group" "ecs" {
  #properties omitted
}

module "ecs_instance_draining_on_scale_in" {
  source = "github.com/terraform-community-modules/tf_aws_ecs_instance_draining_on_scale_in"

  autoscaling_group_name = "${aws_autoscaling_group.ecs.asg_name}"
  hook_heartbeat_timeout = 1800
  hook_default_result    = "ABANDON"
}
```

Author
------
Created and maintained by [Shayne Clausson](https://github.com/sclausson)
