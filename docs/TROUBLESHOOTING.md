# Troubleshooting

## How to debug CrashLoopBackOff

Errors reported by kubelet on master:

```bash
journalctl -u kubelet
```

More logs:

```
kubectl logs [POD NAME] -p
```

the -p option will read the logs of the previous (crashed) instance
