==Dear Students,==
 
==I've just released coursework 1 via Learn. You can access all the materials on the EIDF by cloning the coursework repository with:==
 
==git clone /home/eidf018/eidf018/shared/wave-cw-26==
 
==If anything needs clarification please get in touch.==
 
==One of you messaged earlier pointing out a mistake in the make submission script, for which I'm grateful! (FYI, I had left the git hash I was comparing against pointing to an old version of the code which isn't in the main branch).==
 
==To get the updated version, you will need to go a git pull. The only change is to the file `make_submission.sh` so you should be safe to run that even with uncommitted changes, although personally I would commit or stash them first.==
 
==If you think you're found another problem, please let me know ASAP.==
 
Thanks to two of you for the bug reports! I had got my loop bounds incorrect on the checking-the-answer bit of the code. I have pushed an update to this (and remembered to also update the make submission script this time)
 
Please do a git pull to update your repositories. (Again, I would recommend committing or stashing your work but in principle a simple git pull should work)￼￼￼  
cw1/  
wave-cw-26/ # 作业仓库（clone 得到）  
builds/  
dev/ # 你自己的开发 build 目录（RelWithDebInfo）  
rel/ # 用于接近批改条件的 Release build  
runs/  
logs/ # 运行输出（stdout/stderr）  
outputs/ # HDF5 输出（如果需要保留）  
scripts/  
run_small.sh # 小规模 correctness 回归脚本  
run_perf_sweep.sh # 多规模性能扫测脚本（只跑，不改代码）  
notes/  
commands.md # 你所有可复现命令记录  
perf.csv # 你整理出来的性能数据（后面画图/写报告）  
report/  
report.pdf # 最终报告（自己写）  
figures/ # 图表素材