
def doShell(path):
    if not os.path.exists(path):
        print("failed， because not find ", path)
        return 1
    os.chmod(path, stat.S_IRWXU)
    code=os.system(path)
    if not os.WIFEXITED(code):
        print(path, " exit unusual")
        return 1
    return os.WEXITSTATUS(code)

        try:
            p = subprocess.check_output(f"echo -n {verison} | sha256sum ",stderr=subprocess.STDOUT,timeout=1,shell=True)
            return p.decode('utf8').strip()[:8]
        except subprocess.TimeoutExpired as time_e:
            print(time_e)
        except subprocess.CalledProcessError as call_e:
            print(call_e.output.decode(encoding="utf-8"))
        return ""

https://blog.csdn.net/qdPython/article/details/127689439
https://blog.csdn.net/zhoumoon/article/details/118753926
