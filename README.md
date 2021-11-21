
<!doctype html>
<html>
<head>

	<title>AFM++</title>
	<meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1">
	<link href="css/main.css" media="screen" rel="stylesheet" type="text/css"/>
	<link href="css/index.css" media="screen" rel="stylesheet" type="text/css"/>
	<link href='https://fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
	<link href='https://fonts.googleapis.com/css?family=Open+Sans+Condensed:400,700' rel='stylesheet' type='text/css'>
	<link href='https://fonts.googleapis.com/css?family=Raleway:400,600,700' rel='stylesheet' type='text/css'>
	<script type="text/x-mathjax-config">
			MathJax.Hub.Config({
			CommonHTML: { linebreaks: { automatic: true } },
			"HTML-CSS": { linebreaks: { automatic: true } },
				 SVG: { linebreaks: { automatic: true } }
			});
	</script>
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
	</script>
	
</head>

<body>

<div class="menu-container noselect">
	<div class="menu">
		<table class="menu-table">
			<tr>
				<td>
					<div class="logo">
						<a href="javascript:void(0)">AFM++</a>
					</div>
				</td>
				<td>
					<div class="menu-items">
						<a class="menu-highlight">Overview</a>
						<a href="https://github.com/cherubicxn/afmplusplus">GitHub</a>
					</div>
				</td>
			</tr>
		</table>
	</div>
</div>

<div class="content-container">
	<div class="content">
		<table class="content-table">
		      <h1 style="text-align:center; margin-top:60px; font-weight: bold; font-size: 35px;">
				Learning Regional Attraction for Line Segment Detection</h1>
	        
				<p style="text-align:center; margin-bottom:15px; margin-top:20px; font-size: 18px;">
					<a href="https://cherubicxn.github.io/" style="color: #0088CC">Nan Xue<script type="math/tex">^1</script>, </a>
					<a href="http://songbai.site/" style="color: #0088CC">Song Bai<script type="math/tex">^2</script>, </a>
					<a href="javascript:void(0)" style="color: #0088CC">Fudong Wang<script type="math/tex">^1</script>, </a>
					<a href="http://captain.whu.edu.cn/xia_En.html" style="color: #0088CC">Gui-Song Xia<script type="math/tex">^{1, *}</script>, </a>
					<a href="https://tfwu.github.io/" style="color: #0088CC">Tianfu Wu<script type="math/tex">^3</script>,</a> 
					<a href="javascript:void(0)" style="color: #0088CC">Liangpei Zhang<script type="math/tex">^1</script>,</a> 
					<a href="http://www.robots.ox.ac.uk/~phst/" style="color: #0088CC">Philip H.S. Torr<script type="math/tex">^2</script></a> <br>
					<!-- <a href="http://www.dsi.unive.it/~pelillo/" style="color: #0088CC">Marcello Pelillo<script type="math/tex">^3</script></a> <br> -->
                     
					<a target="_blank" href="http://captain.whu.edu.cn/" style="color: #0088CC; font-style: italic"><script type="math/tex">^1</script>LIESMARS CAPTAIN, Wuhan University, Wuhan, China</a><br>
					<a target="_blank" href="http://en.whu.edu.cn/" style="color: #0088CC; font-style: italic"><script type="math/tex">^2</script>University of Oxford, Oxford, United Kingdom</a>  <br>
					<a target="_blank" href="https://www.ece.ncsu.edu/" style="color: #0088CC; font-style: italic"><script type="math/tex">^3</script>Dept. Electrical & Computer Engineering, NC State University, USA</a> 

				</p>	 
				<p style="text-align:center; margin-bottom:15px; margin-top:20px; font-size: 18px;"> 
					<a> The source code will be released soon!
					</a>
				</p>
		
			
			<tr>
				<!-- <td colspan="1"> -->
					<h2 class="add-top-margin">Abstract</h2>
					<hr>
				<!-- </td> -->
			</tr>
			<tr>
					<!-- <td colspan="1"> -->

				<p class="text" style="text-align:justify;">
					This paper presents regional attraction of line segment maps, and hereby poses the problem of line segment detection (LSD) as a problem of region coloring.
					Given a line segment map, the proposed regional attraction first establishes the relationship between line segments and regions in the image lattice. 
					Based on this, the line segment map is equivalently transformed to an attraction field map (AFM), which can be remapped to a set of line segments without loss of information. 
					Accordingly, we develop an end-to-end framework to learn attraction field maps for raw input images, followed by a squeeze module to detect line segments. 
					Apart from existing works, the proposed detector properly handles the local ambiguity and does not rely on the accurate identification of edge pixels.
					Comprehensive experiments on the Wireframe dataset and the YorkUrban dataset demonstrate the superiority of our method. 
					In particular, we achieve an F-measure of 0.831 on the Wireframe dataset, advancing the state-of-the-art performance by 10.3 percent.	
				</p>
					<!-- </td> -->
			</tr>

			<tr>
				<!-- <td colspan="1"> -->
					<h2 class="add-top-margin">Introduction</h2>
					<hr>
				<!-- </td> -->
			</tr>

			<tr>
				<p class="text" style="text-align:justify;">
					Line segment detection (LSD) is an important yet challenging low-level task in computer vision. 
					LSD aims to extract visible line segments in scene images (See Figure 1 (a) and (e)). 
					Most of the existing line segment detectors are built upon edge pixel identification  with two main drawbacks:
					such work lacks elegant solutions to solve the issues caused by inaccurate or incorrect edge detection results (<it>e.g.</it>, local ambiguity, high false positive detection rates and incomplete line segments) and requires carefully designed heuristics or extra contextual information to infer line segments from identified edge pixels.
				</p>
			</tr>

			<tr>
				<td>
					<div>
					<img class="center" src="img/teaser.png" width="900" /> <br>
					<p class="image-caption">
						Figure 1. Illustrative examples of different methods on an image. In the top row: (a) shows an example test image in the Wireframe dataset; 
						(b)-(e) shows the corresponding local edge map, gradient magnitude and deep edge map respectively. 
						In the bottom row: (e) shows our detected line segments; (f) - (h) display the results of MCMLSD [1], LSD [2] and DWP [3] respectively.
					</p>				
					</div>
				</td>
			</tr>
			
			<tr>
				<td>
				<p class="text" style="text-align:justify;">
					In this paper, we focus on a deep learning based LSD framework and propose a single-stage method that rigorously addresses the drawbacks of existing LSD approaches.
					Our method is motivated by the following observations:
					<ul style="font-weight:normal; text-align:justify;">
							<li > The duality between regions and the contour (or the surface) of an object is well-known in computer vision. </li>
							<li> All pixels in the image lattice should be involved in the formation of line segments in an image. </li>
							<li> The recent remarkable progress led by deep learning based methods (e.g., U-Net and DeepLab V3+) in semantic segmentation.</li>							
					</ul>

					Thus, the intuitive idea of this paper is that when bridging a line segment map and its spatial proximate regions, we can pose the problem of LSD as the problem of region coloring, and thus open the door to leveraging the best practices developed in state-of-the-art deep ConvNet based semantic segmentation methods to improve the performance of LSD.

					Following this idea, we exploit the spatial relationship between pixels in the image lattice and line segments, and propose a new formulation termed regional attraction for line segment detection.
					By learning the regional attraction, our proposed line segment detector eliminates the limitations of edge pixel identification. 
				</p>
				</td>
			</tr>

			<tr>
				<td>
						<h2 class="add-top-margin">Experimental Results</h2>
						<hr>
				</td>
			</tr>

			<tr>
					<td>
							<h3 class="add-top-margin">Results on Wireframe Dataset</h3>
							
					</td>
				</tr>
					
			<tr>
				<td>
					<div>
					<img class="center" src="img/Results_on_wireframe.svg" width="900" /> <br>
					<p class="image-caption">
						Figure 2. Some results of line segment detection of different approaches on the Wireframe dataset. From top to bottom: LSD [4], MCMLSD [3], Linelet [6], DWP [5], AFM [2] with the a-trous Residual U-Net and AFM++ proposed in this paper.
					</p>				
					</div>
				</td>
			</tr>
			<tr>
					<td>
							<h3 class="add-top-margin">Results on YorkUrban Dataset</h3>
							
					</td>
				</tr>
			<tr>
				<td>
					<div>
					<img class="center" src="img/Results_on_yorkurban.svg" width="900" /> <br>
					<p class="image-caption">
							Figure 3. Some results of line segment detection of different approaches on the YorkUrban dataset. From top to bottom: LSD [4], MCMLSD [3], Linelet [6], DWP [5], AFM [2] with the a-trous Residual U-Net and AFM++ proposed in this paper.
					</p>				
					</div>
				</td>
			</tr>

			<tr>
					<td colspan="2">
					<h2 class="add-top-margin">References</h2>
					<hr>
					<ol style="padding-inline-start:20px;">
						<li>
							<b>Learning Regional Attraction for Line Segment Detection </b>
							[<a href="https://ieeexplore.ieee.org/document/8930083" style="color: #0088CC">paper</a>]<br>
								N. Xue, S. Bai, F.-D. Wang, G.-S. Xia, T. Wu, L. Zhang, P.H.S. Torr <br>
								IEEE Trans. on Pattern Analysis and Machine Intelligence (<b>TPAMI</b>), 2019.
						</li>	
						<li>
							<b>Learning Attraction Field Representation for Robust Line Segment Detection </b>
							[<a href="http://openaccess.thecvf.com/content_CVPR_2019/papers/Xue_Learning_Attraction_Field_Representation_for_Robust_Line_Segment_Detection_CVPR_2019_paper.pdf" style="color: #0088CC">paper</a>]<br>
							   N. Xue, S. Bai, F.-D. Wang, G.-S. Xia, T. Wu, L. Zhang <br>
							   IEEE Conference on Computer Vision and Pattern Recognition (<b>CVPR</b>), 2019.
						</li>	

						<li>
								<b>MCMLSD: A Dynamic Programming Approach to Line Segment Detection</b>
								[<a href="https://ieeexplore.ieee.org/document/8100103" style="color: #0088CC">paper</a>]<br>
								E. J. Almazan,  R. Tal,  Y. Qian, J. H. Elder<br>
								IEEE Conference on Computer Vision and Pattern Recognition (<b>CVPR</b>), 2017.
						</li>	

						<li>
								<b>LSD: A Fast Line Segment Detector with a False Detection Control</b>
								[<a href="https://ieeexplore.ieee.org/abstract/document/4731268" style="color: #0088CC">paper</a>]<br>
								R. G. von Gioi, J. Jakubowicz, J. M. Morel, G. Randall <br>
									IEEE Trans. on Pattern Analysis and Machine Intelligence (<b>TPAMI</b>), 2010.
						</li>	

						<li>
							<b>Learning to Parse Wireframes in Images of Man-Made Environments</b>
							[<a href="http://openaccess.thecvf.com/content_cvpr_2018/papers/Huang_Learning_to_Parse_CVPR_2018_paper.pdf" style="color: #0088CC">paper</a>]<br>
							K. Huang, Y. Wang, Z. Zhou, T. Ding, S. Gao, Y. Ma<br>
							IEEE Conference on Computer Vision and Pattern Recognition (<b>CVPR</b>), 2018.
						</li>	


						<li>
								<b>A Novel Linelet-Based Representation for Line Segment Detection</b>
								[<a href="https://ieeexplore.ieee.org/abstract/document/7926451" style="color: #0088CC">paper</a>]<br>
								N.-G. Cho, A. Yuille, S.-W. Lee<br>
									IEEE Trans. on Pattern Analysis and Machine Intelligence (<b>TPAMI</b>), 2018.
							</li>	

					</ol>
					</td>
				</tr>
			<br><br>
		 </table>
		 			
		
	<div class="footer">
		<p class="block">&copy; 2019 by Nan Xue at CAPTAIN</p>
	</div>

	</div>
</div>
</body>
</html>
