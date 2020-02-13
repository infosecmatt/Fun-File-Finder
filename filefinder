#! /usr/bin/python3
from argparse import *
from urllib.parse import urlparse
from requests import get

#Create parser
parser = ArgumentParser(description='Search for specific files within a given list of directories.')

#Create mutually exclusive groups
dirs = parser.add_mutually_exclusive_group(required=True)
files = parser.add_mutually_exclusive_group(required=True)
#Add arguments
parser.add_argument('--url', help='URL to enumerate', required=True)
dirs.add_argument('--dirs', help='Names of the directories to search in. Separate each directory with a space.',nargs='+')
dirs.add_argument('--dirlist', help='Name of file that contains list of directories. Ensure that each directory is on its own line.')
files.add_argument('--files', help='Names of the files to search for. Separate each name with a space.', nargs='+')
files.add_argument('--filelist', help='Name of the file that contains list of files. Ensure that each file is on its own line.')
parser.add_argument('--home', help='Search home folder in addition to listed directories',action='store_true')
parser.add_argument('--status', help='HTTP status codes to return. Separate with spaces. All codes are returned by default.', nargs='+', type=int)

#Parse arguments
args = parser.parse_args()

#Store input in variables
u = urlparse(args.url)
url = u.scheme + '://' + u.netloc
status = args.status

if args.dirs is not None:
	dirs = args.dirs
else:
	with open(args.dirlist) as x:
		dirs = [line.rstrip('\n') for line in x]
if args.files is not None:
	files = args.files
else:
	with open(args.filelist) as y:
		files = [line.rstrip('\n') for line in y]
if args.home is not None:
	dirs.insert(0,"/")



#Search for files
for file in files:
	for dir in dirs:
		dir = dir.strip('/')
		dir = '/' + dir
		path = url + dir + '/' + file
		response = get(path)
		ignore = False #should this output be ignored?
		if status is not None:
			ignore = True
			for code in status:
				if response.status_code == code:
					ignore = False					
					break
		if ignore == False:					
			print(str(response.status_code) + ": " + path)
