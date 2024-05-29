

```python


from argparse import ArgumentParser

def gyt_argparse(host, filename, iface):
	print(host)
	print(filename)
	print(iface)

usage = "usage: python arg_parse.py -t host -f filename -i interface"

parser = ArgumentParser(usage=usage)
# 基于参数的
# 一个短参数, 一个常参数
parser.add_argument("-f", "--file", dest="filename", help="Write content to FILE", dafault="1.txt", type=str)

parser.add_argument("-i", "--interface", dest="iface", help="Specify an interface" dafault=1, type=int)
parser.add_argument("-h", "--host", dest="host", help="Specify an host", dafault="10.1.1.1",type=str )


# 基于位置的, 至多有一个
parser.add_argument(nargs='?' dest="host", help="Specify an host", dafault="10.1.1.1",type=str )
# 基于多个位置
parser.add_argument(nargs='*' dest="host", help="Specify an host", dafault="10.1.1.1",type=str )



args = parser.parse_args()
					
qyt_argparse(args.hosts, args.filename, args.iface)


```