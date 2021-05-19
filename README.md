# Pre build windows 10 Swar's Chia Plot Manager 

#### A plot manager for Chia plotting: https://www.chia.net/
[English](README.md)


#### You can run this plotter in a brand new computer with just one command

![start and stop the manager](https://github.com/don-lin/Windows10-Chia-Plot-Manager/blob/main/screenshot/start_stop.png?raw=true)



##### Development Version: v0.0.1

This is a cross-platform Chia Plot Manager that will work on the major operating systems. This is not a plotter. The purpose of this library is to manage your plotting and kick off new plots with the settings that you configure. Everyone's system is unique so customization is an important feature that was engraved into this library.

This library is simple, easy-to-use, and reliable to keep the plots generating.

This library has been tested for Windows and Linux.


## Features

* Stagger your plots so that your computer resources can avoid high peaks.
* Allow for a list of destination directories.
* Utilize temporary space to its maximum potential by starting a new plot early.
* Run a maximum number of plots concurrently to avoid bottlenecks or limit resource hogging.
* More in-depth active plot screen.

## Frequently Asked Questions

##### Can I reload my config?
* Yes, your config can be reloaded with the `python manager.py restart` command or separately you can stop and start manager again. Please note that your job counts will be reset and the temporary2 and destination directories order will be reset.
* Please note that if you change any of the directories for a job, it will mess with existing jobs and `manager` and `view` will not be able to identify the old job. If you are changing job directories while having active plots, please change the `max_plots` for the current job to 0 and make a separate job with the new directories. I **do not recommend** changing directories while plots are running.

##### If I stop manager will it kill my plots?
* No. Plots are kicked off in the background and they will not kill your existing plots. If you want to kill them, you have access to the PIDs which you can use to track them down in Task Manager (or the appropriate software for your OS) and kill them manually. Please note you will have to delete the .tmp files as well. I do not handle this for you.

##### How are temporary2 and destination selected if I have a list?
* They are chosen in order. If you have two directories the first plot will select the first one, the second the second one, and the third plot will select the first one.

##### What is `temporary2_destination_sync`?
* Some users like having the option to always have the same temporary2 and destination directory. Enabling this setting will always have temporary2 be the drive that is used as destination. You can use an empty temporary2 directory list if you are using this setting.

##### What is the best config for my setup?
* Please forward this question to Keybase or the Discussion tab.


## Build from souce (not mandatory)

The installation of this library is straightforward. I have attached detailed instructions below that should help you get started. 

1. Download and Install Python 3.8 or higher: https://www.python.org/
2. `git clone` this repo or download it.
3. Open CommandPrompt / PowerShell / Terminal and `cd` into the main library folder.
   * Example: `cd C:\Users\Donlin\Documents\Swar-Chia-Plot-Manager`
5. Install the required modules: `pip install -r requirements.txt`
6. `pyinstaller -F manager.py`
7. `pyinstaller -F stateless-manager.py`



## Configuration

The configuration of this library is unique to every end-user. The `config.yaml` file is where the configuration will live. 

This plot manager works based on the idea of jobs. Each job will have its own settings that you can configure and customize. No two drives are unique so this will provide flexibility for your own constraints and requirements.

### manager

These are the config settings that will only be used by the plot manager.

* `check_interval` - The number of seconds to wait before checking to see if a new job should start.
* `log_level` - Keep this on ERROR to only record when there are errors. Change this to INFO in order to see more detailed logging. Warning: INFO will write a lot of information.

### log

* `folder_path` - This is the folder where your log files for plots will be saved.

### view

These are the settings that will be used by the view.

* `check_interval` - The number of seconds to wait before updating the view.
* `datetime_format` - The datetime format that you want displayed in the view. See here for formatting: https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes
* `include_seconds_for_phase` - This dictates whether seconds are included in the phase times.
* `include_drive_info` - This dictates whether the drive information will be showed.
* `include_cpu` - This dictates whether the CPU information will be showed.
* `include_ram` - This dictates whether the RAM information will be showed.
* `include_plot_stats` - This dictates whether the plot stats will be showed.

### notifications

These are different settings in order to send notifications when the plot manager starts and when a plot has been completed.

### progress

* `phase_line_end` - These are the settings that will be used to dictate when a phase ends in the progress bar. It is supposed to reflect the line at which the phase will end so the progress calculations can use that information with the existing log file to calculate a progress percent. 
* `phase_weight` - These are the weight to assign to each phase in the progress calculations. Typically, Phase 1 and 3 are the longest phases so they will hold more weight than the others.

### global
* `max_concurrent` - The maximum number of plots that your system can run. The manager will not kick off more than this number of plots total over time.

### job

These are the settings that will be used by each job. Please note you can have multiple jobs and each job should be in YAML format in order for it to be interpreted correctly. Almost all the values here will be passed into the Chia executable file. 

Check for more details on the Chia CLI here: https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference

* `name` - This is the name that you want to give to the job.
* `max_plots` - This is the maximum number of jobs to make in one run of the manager. Any restarts to manager will reset this variable. It is only here to help with short term plotting.
* [OPTIONAL]`farmer_public_key` - Your farmer public key. If none is provided, it will not pass in this variable to the chia executable which results in your default keys being used. This is only needed if you have chia set up on a machine that does not have your credentials.
* [OPTIONAL]`pool_public_key` - Your pool public key. Same information as the above. 
* `temporary_directory` - Only a single directory should be passed into here. This is where the plotting will take place.
* [OPTIONAL]`temporary2_directory` - Can be a single value or a list of values. This is an optional parameter to use in case you want to use the temporary2 directory functionality of Chia plotting.
* `destination_directory` - Can be a single value or a list of values. This is the final directory where the plot will be transferred once it is completed. If you provide a list, it will cycle through each drive one by one.  
* `size` - This refers to the k size of the plot. You would type in something like 32, 33, 34, 35... in here.
* `bitfield` - This refers to whether you want to use bitfield or not in your plotting. Typically, you want to keep this as true.
* `threads` - This is the number of threads that will be assigned to the plotter. Only phase 1 uses more than 1 thread.
* `buckets` - The number of buckets to use. The default provided by Chia is 128.
* `memory_buffer` - The amount of memory you want to allocate to the process.
* `max_concurrent` - The maximum number of plots to have for this job at any given time.
* `max_concurrent_with_start_early` - The maximum number of plots to have for this job at any given time including phases that started early.
* `stagger_minutes` - The amount of minutes to wait before the next plot for this job can get kicked off. You can even set this to zero if you want your plots to get kicked off immediately when the concurrent limits allow for it.
* `max_for_phase_1` - The maximum number of plots on phase 1 for this job.
* `concurrency_start_early_phase` - The phase in which you want to start a plot early. It is recommended to use 4 for this field.
* `concurrency_start_early_phase_delay` - The maximum number of minutes to wait before a new plot gets kicked off when the start early phase has been detected.
* `temporary2_destination_sync` - This field will always submit the destination directory as the temporary2 directory. These two directories will be in sync so that they will always be submitted as the same value.
