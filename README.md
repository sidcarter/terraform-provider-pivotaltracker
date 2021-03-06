# terraform-provider-pivotaltracker
a terraform provider to manage pivotal tracker projects and membership

[![CircleCI](https://circleci.com/gh/xchapter7x/terraform-provider-pivotaltracker/tree/master.svg?style=svg)](https://circleci.com/gh/xchapter7x/terraform-provider-pivotaltracker/tree/master)
[![Go Report Card](https://goreportcard.com/badge/github.com/xchapter7x/terraform-provider-pivotaltracker)](https://goreportcard.com/report/github.com/xchapter7x/terraform-provider-pivotaltracker)
[![GoDoc](https://godoc.org/github.com/xchapter7x/terraform-provider-pivotaltracker?status.svg)](https://godoc.org/github.com/xchapter7x/terraform-provider-pivotaltracker)


### Installing the Plugin

```bash
# create the providers dir if needed
$ mkdir -p ~/terraform-providers

# set your version (most recent is here: https://github.com/xchapter7x/terraform-provider-pivotaltracker/releases/latest )
$ export VERSION=v0.0.2
# set your platform (unix|osx|win) supported
$ export PLATFORM=osx

# dload the plugin
$ curl -L https://github.com/xchapter7x/terraform-provider-pivotaltracker/releases/download/${VERSION}/pivotal_tracker_provider_${PLATFORM} -o ~/terraform-providers/pivotal_tracker_provider

# make it executable just in case
$ chmod +x ~/terraform-providers/pivotal_tracker_provider

# add the provider to your terraformrc (create one like below if one doesnt exist)
$ cat << EOF > ~/.terraformrc
providers {
    pivotaltracker = "$HOME/terraform-providers/pivotal_tracker_provider"
}
EOF

# create a sample project to manage with terraform
$ cat << EOF > sampleProject.tf 
resource "pivotaltracker_project" "test_project" {
   name  = "some_new_project"
   description = "change description again"
}
EOF

# initialize plugins
$ terraform init

# set your tracker API TOKEN (found at the bottom of your profile page https://www.pivotaltracker.com/profile )
$ export PVTL_TRACKER_TOKEN=xxxxx....

# check your terraform
$ terraform plan

# and have terraform do its thing
$ terraform apply
```


### Available Resources
- Members (COMING SOON)
- Project Membership (COMING SOON)
- Project Resource [Tracker API Docs](https://www.pivotaltracker.com/help/api/rest/v5#Project)
  - fields:
  
    	"no_owner": `boolean in the request body.
				 —  By default, the user whose credentials are supplied 
				 will be added as a project owner. To leave the project 
				 without this owner, supply the no_owner key.`

		"new_account_name": `string[100] in the request body.
				 —  If specified, creates a new account with the specified 
				 name, and adds the new project to that account.`

		"name": `extended string[50] in the request body.
				 —  The name of the project.`

		"status": `string in the request body.
				 —  The status of the project.`

		"iteration_length": `int in the request body.
				 —  The number of weeks in an iteration.`

		"week_start_day": `enumerated string in the request body.
				 —  The day in the week the project's iterations are
				 to start on.	Valid enumeration values: Sunday, Monday, 
				 Tuesday, Wednesday, Thursday, Friday, Saturday`

		"point_scale": `string[255] in the request body.
				 —  The specification for the "point scale" available 
				 for entering story estimates within the project. It 
				 is specified as a comma-delimited series of values--any 
				 value that would be acceptable on the Project Settings 
				 page of the Tracker web application may be used here. 
				 If an exact match to one of the built-in point scales, 
				 the project will use that point scale. If another 
				 comma-separated point-scale string is passed, it will 
				 be treated as a "custom" point scale. The built-in 
				 scales are "0,1,2,3", "0,1,2,4,8", and "0,1,2,3,5,8".`

		"bugs_and_chores_are_estimatable": `boolean in the request body.
				 —  When true, Tracker will allow estimates to be set 
				 on Bug- and Chore-type stories. This is strongly not 
				 recommended. Please see the FAQ for more information.`

		"automatic_planning": `boolean in the request body.
				 —  When false, Tracker suspends the emergent planning of 
				 iterations based on the project's velocity, and allows 
				 users to manually control the set of unstarted stories 
				 included in the Current iteration. See the FAQ for more information.`

		"enable_tasks": `boolean in the request body.
				 —  When true, Tracker allows individual tasks to be 
				 created and managed within each story in the project.`

		"start_date": `date in the request body.
				 —  The first day that should be in an iteration of the 
				 project. If both this and "week_start_day" are supplied,
				 they must be consistent. It is specified as a string in 
				 the format "YYYY-MM-DD" with "01" for January. If this is 
				 not supplied, it will remain blank (null), but "start_time"
				 will have a default value based on the stories in the project.
				 If a value is supplied for start_date, but that date is 
				 later than the accepted_at date of the earliest accepted 
				 story in your project, start_time will be based on the 
				 accepted_at date of the earliest accepted story.`

		"time_zone": `time_zone in the request body.  —  The "native" time zone for the
				project, independent of the time zone(s) from which members of the
				project view or modify it.`

		"velocity_averaged_over": `int in the request body.  —  The number of iterations that should be used when
				averaging the number of points of Done stories in order to compute the
				project's velocity.`

		"number_of_done_iterations_to_show": `int in the request body.  —  There are areas within the Tracker UI and the API
				in which sets of stories automatically exclude the Done stories contained in
				older iterations. For example, in the web UI, the DONE panel doesn't
				necessarily show all Done stories by default, and provides a link to click to
				cause the full story set to be loaded/displayed. The value of this attribute is
				the maximum number of Done iterations that will be loaded/shown/included in
				these areas.`

		"description": `extended string[140] in the request body.  —  A description of the
				project's content. Entered through the web UI on the Project Settings
				page.`

		"profile_content": `extended string[65535] in the request body.  —  A long description of
				the project. This is displayed on the Project Overview page in the
				Tracker web UI.`

		"enable_incoming_emails": `boolean in the request body.  —  When true, the project will accept
				incoming email responses to Tracker notification emails and convert
				them to comments on the appropriate stories.`

		"initial_velocity": `int in the request body.  —  The number which should be used as the
				project's velocity when there are not enough recent iterations with
				Done stories for an actual velocity to be computed.`

		"project_type": `enumerated string in the request body.  —  The project's type which
				determines visibility and permissions [demo is deprecated].  Valid
				enumeration values: demo, private, public, shared`

		"public": `boolean in the request body.  —  When true, Tracker will allow any user
				on the web to view the content of the project. The project will not
				count toward the limits of a paid subscription, and may be included on
				Tracker's Public Projects listing page.`

		"atom_enabled": `boolean in the request body.  —  When true, Tracker allows people to
				subscribe to the Atom (RSS, XML) feed of project changes.`

		"account_id": `int in the request body.  —  The ID number for the account which
				contains the project.`

		"join_as": `enumerated string in the request body.  —  The default join_as value
				for the project [viewer, member].  Valid enumeration values: owner,
				member, viewer
 
				The new project is created with the currently-authenticated user as its
				original Owner. The server will supply a default value for any optional
				parameter that the request doesn't include. The default values are not defined
				as part of the API--they may change from time to time without an increase in
				the current API version number. Additionally, new project attributes may be
				added at any time without advance notice. The client will know what values the
				server has supplied from the response to the request. If the value set for a
				project attribute is important to an application, it should be included
				explicitly in the request without relying on the server to supply the
				default.`

