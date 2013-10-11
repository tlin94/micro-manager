#!/usr/bin/python
import SimpleHTTPServer
import SocketServer
import sys, shutil ,subprocess, os, tempfile
import xml.etree.ElementTree as ET
import base64
PATH_MICRO = "/home/tlin/Micro"

class MyRequestHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):
		def do_GET(self):
					if self.path == '/':
						self.send_response(200)
						self.send_header('Content-type','text/html')
						self.end_headers()
						return 

					elif self.path == "/index.html":
						SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)
						'''self.send_error(404, "requested path not available")'''
						return
	 			
		def do_POST(self) :	
					if self.path == "/receiveXML.html" :						
						# get XML message
						length = int(self.headers.getheader('content-length')) 
						data = self.rfile.read(length)
						'''print "The xml content is %s" %data '''
						root = ET.fromstring(data)
						# run Simulation
						if root[2].text == 'StartSimulation' :
							self.wfile.write("<h2> Running Simulation with %s Processors</h2>" % root[3].text)
							SimFunction =  MySimFunction ()
							SimFunction.startSim(root[3].text, root[0].text, root[1].text)
							self.wfile.write("<h2> Simulation finished")
							return
					
class MySimFunction :
	def startSim(self, nProcessors, text1, text2):
		
		print "Start Sim with %s processors ..." %nProcessors 				
		
		#create a folder for the simulation results and add copy run.sh
		tempDir = tempfile.mkdtemp('','Sim_',PATH_MICRO + '/Temp')
		shutil.copy2(PATH_MICRO + '/run.sh',tempDir)
		os.chdir(tempDir) 
		
		# create two input files
		material_input = open ('material_input.txt','w')
		dislocation_input = open ('interaction_geom_input.txt','w')
		material_input.write(base64.b64decode(text1))
		dislocation_input.write(base64.b64decode(text2))
		
		# add the path of micro to the env variable "PATH" 
		os.environ["PATH"] += os.pathsep  + PATH_MICRO
		subprocess.Popen(tempDir + "/run.sh %s" %nProcessors, shell=True, env=os.environ)	
			
		
	def stopSim(self):
		print "Stop Simualtion"
		
		
	def funtest(self):	
		print "funtest"
				

		
		
# set the Socket Server
Handler = MyRequestHandler
server = SocketServer.TCPServer(('0.0.0.0', 8080), Handler)
print("Server listening on port 8080...")
server.serve_forever()

