# Deepspeed notes



## Should I use the `deepspeed` launcher under slurm.

No, it won't work.

Instead use:
```
python -u -m torch.distributed.launch \
    --nproc_per_node $GPUS_PER_NODE \
    --nnodes $NNODES \
    --master_addr $MASTER_ADDR \
    --master_port $MASTER_PORT \
    --node_rank $SLURM_PROCID \
    ....
```

## on 8 gpus I get now: `data_parallel_size: 8, parameter_parallel_size: 8`

In this case seeing that the DP and parameter parallel size match means ZeRO will partition across all gpus



## experiments


- try to tune up the max micro-bs on 1 node model scaled down to a few layers (Same hidden size)
- experiment in the range of 16 to 64 to get the highest tflops
- how efficient it's running w/o communications
- fit on a single node
- could turn off optimizer step - no communications between gpus
- one more hyper param to experiment with:
  tiled - turn it on - overlapping communication improvement