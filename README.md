  
 1. Create Bucket  
 2. Upload object - cat.jpg  
 3. Bucket Settings >> Permissions >> Update Bucket Policy  
 ''' 
 { 
     "Version": "2012-10-17", 
     "Statement": [ 
         { 
             "Sid": "PublicReadGetObject", 
             "Effect": "Allow", 
             "Principal": "*", 
             "Action": "s3:GetObject", 
             "Resource": "arn:aws:s3:::mybucket9584417244/*" 
         } 
     ] 

 }
 '''
 4. Copy Object URL >> Paste in browser  
 5. launch EC2 - SSH  
 SG - http, https, ssh 
 ... 
  sudo yum update -y  
 sudo yum install httpd -y  
 sudo systemctl start httpd  
 sudo systemctl enable httpd  
 cd /var/www/html  
 sudo chmod 755 /var/www/html  
 sudo touch index.html  
 sudo nano index.html  
...  
 6. copy and paste index.html code containing S3 Object URL  
 ...
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Canvas Gallery | Original Fine Art</title>
    <style>
        :root {
            --bg-color: #fbfbfc;
            --text-main: #1a1a1a;
            --text-muted: #757575;
            --accent: #b8926a; /* Gallery gold/bronze */
            --card-bg: #ffffff;
            --border: #e5e5e5;
        }

        body {
            font-family: 'Didot', 'Bodoni MT', 'Cinzel', 'Georgia', serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        /* Navigation Bar */
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1.5rem 5%;
            border-bottom: 1px solid var(--border);
            background: #fff;
        }
        .logo {
            font-size: 1.5rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            font-weight: bold;
        }
        .nav-links {
            list-style: none;
            display: flex;
            gap: 2rem;
            margin: 0;
            padding: 0;
        }
        .nav-links a {
            text-decoration: none;
            color: var(--text-main);
            font-family: sans-serif;
            font-size: 0.85rem;
            letter-spacing: 1px;
            text-transform: uppercase;
            transition: color 0.3s;
        }
        .nav-links a:hover { color: var(--accent); }

        /* Hero Header */
        header {
            text-align: center;
            padding: 5rem 1rem 3rem 1rem;
            max-width: 800px;
            margin: 0 auto;
        }
        header h1 {
            font-size: 3rem;
            font-weight: 300;
            margin-bottom: 1rem;
            letter-spacing: 2px;
        }
        header p {
            font-family: sans-serif;
            color: var(--text-muted);
            font-size: 1.1rem;
        }

        /* Container & Responsive Gallery Grid */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem 5% 6rem 5%;
        }
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 3rem;
        }

        /* Painting Cards */
        .art-card {
            background-color: var(--card-bg);
            border: 1px solid var(--border);
            border-radius: 4px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0,0,0,0.02);
            transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s;
        }
        .art-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 12px 24px rgba(0,0,0,0.08);
        }
        
        .image-container {
            position: relative;
            background-color: #eaeaea;
            overflow: hidden;
            aspect-ratio: 4 / 5; /* Elegant portrait crop */
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .image-container img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.6s ease;
        }
        
        /* Fallback format configuration */
        .image-container img.fallback-art {
            width: 85%;
            height: 85%;
            object-fit: contain;
            border-radius: 2px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.15);
        }

        .art-card:hover .image-container img {
            transform: scale(1.04);
        }

        /* Art Details Frame */
        .art-info {
            padding: 1.5rem;
            text-align: center;
        }
        .art-title {
            font-size: 1.3rem;
            margin: 0 0 0.25rem 0;
            font-weight: 400;
            letter-spacing: 1px;
        }
        .art-meta {
            font-family: sans-serif;
            font-size: 0.85rem;
            color: var(--text-muted);
            margin-bottom: 1rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .art-price {
            font-size: 1.25rem;
            color: var(--text-main);
            font-weight: 600;
            margin-bottom: 1.2rem;
        }
        
        /* Purchase Button */
        .btn-action {
            display: inline-block;
            width: 100%;
            padding: 0.8rem 0;
            background-color: var(--text-main);
            color: #fff;
            text-decoration: none;
            font-family: sans-serif;
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 2px;
            border-radius: 2px;
            transition: background-color 0.3s;
        }
        .btn-action:hover {
            background-color: var(--accent);
        }

        footer {
            text-align: center;
            padding: 3rem;
            background-color: #fff;
            border-top: 1px solid var(--border);
            font-family: sans-serif;
            font-size: 0.85rem;
            color: var(--text-muted);
            letter-spacing: 1px;
        }
    </style>
    <script>
        // Automatic Error Correction Script
        function handleArtError(imageElement, fallbackUrl) {
            imageElement.onerror = null; 
            imageElement.classList.add('fallback-art');
            imageElement.src = fallbackUrl;
        }
    </script>
</head>
<body>

    <nav>
        <div class="logo">V.C. Studio</div>
        <ul class="nav-links">
            <li><a href="#gallery">Collection</a></li>
            <li><a href="#about">About Artist</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>

    <header>
        <h1>Original Fine Art Collection</h1>
        <p>An expanded showcase of contemporary textures, rich oil studies, and classical expressions. Handcrafted, signed, and ready to install.</p>
    </header>

    <div class="container" id="gallery">
        <div class="gallery-grid">

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #eccc68, #ff7f50);">
                    <img src="https://images.unsplash.com/photo-1579783900882-c0d3dad7b119?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1024/400/500')" 
                         alt="Impressionist Fine Art">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Serenity in Static</h3>
                    <div class="art-meta">Oil on Canvas • 36" x 48"</div>
                    <div class="art-price">$1,250.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Serenity in Static')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #70a1ff, #1e90ff);">
                    <img src="https://images.unsplash.com/photo-1541701494587-cb58502866ab?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1042/400/500')" 
                         alt="Fluid Blue Abstract Painting">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Golden Amber Study</h3>
                    <div class="art-meta">Mixed Media & Resin • 24" x 30"</div>
                    <div class="art-price">$850.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Golden Amber Study')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #2f3542, #747d8c);">
                    <img src="https://images.unsplash.com/photo-1580136579312-94651dfd596d?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1050/400/500')" 
                         alt="Moody Classical Oil Art">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Midnight Nocturne</h3>
                    <div class="art-meta">Oil on Wood Panel • 40" x 40"</div>
                    <div class="art-price">$1,600.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Midnight Nocturne')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #ff4757, #ff6b81);">
                    <img src="https://images.unsplash.com/photo-1547891654-e66ed7edd96c?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1062/400/500')" 
                         alt="Dynamic Vibrant Ink Splatter">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Nebula Shards</h3>
                    <div class="art-meta">Alcohol Ink on Yupo • 18" x 24"</div>
                    <div class="art-price">$450.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Nebula Shards')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #ffeaa7, #fab1a0);">
                    <img src="https://images.unsplash.com/photo-1579783928621-7a13d66a6211?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1069/400/500')" 
                         alt="Soft Pastel Brushwork">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Whisper in Pastel</h3>
                    <div class="art-meta">Watercolor on Coldpress • 16" x 20"</div>
                    <div class="art-price">$320.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Whisper in Pastel')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #a55eea, #4b7bec);">
                    <img src="https://images.unsplash.com/photo-1513364776144-60967b0f800f?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1081/400/500')" 
                         alt="Abstract Contemporary Canvas">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Architectural Alignment</h3>
                    <div class="art-meta">Gouache on Canvas • 30" x 30"</div>
                    <div class="art-price">$980.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Architectural Alignment')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #ff6b6b, #ee5253);">
                    <img src="https://images.unsplash.com/photo-1549887534-1541e9326642?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1015/400/500')" 
                         alt="Crimson Silhouette Abstract Art">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Crimson Silhouette</h3>
                    <div class="art-meta">Acrylic & Charcoal • 24" x 36"</div>
                    <div class="art-price">$920.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Crimson Silhouette')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #10ac84, #1dd1a1);">
                    <img src="https://images.unsplash.com/photo-1557672172-298e090bd0f1?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1035/400/500')" 
                         alt="Textured Marble Fluid Art">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Emerald Dream</h3>
                    <div class="art-meta">Mixed Media on Panel • 30" x 40"</div>
                    <div class="art-price">$1,150.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Emerald Dream')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #57606f, #2f3542);">
                    <img src="https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1040/400/500')" 
                         alt="Dark Minimalist Sculpted Art">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Solitude</h3>
                    <div class="art-meta">Charcoal & Gesso • 20" x 20"</div>
                    <div class="art-price">$510.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Solitude')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #ff9f43, #ee5253);">
                    <img src="https://images.unsplash.com/photo-1561214115-f2f134cc4912?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1053/400/500')" 
                         alt="Bright Colorful Modern Brushwork">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Urban Kinetic</h3>
                    <div class="art-meta">Graffiti & Oil Blend • 36" x 36"</div>
                    <div class="art-price">$1,400.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Urban Kinetic')">Inquire to Purchase</a>
                </div>
            </div>

            <div class="art-card">
                <div class="image-container" style="background: linear-gradient(135deg, #00d2d3, #54a0ff);">
                    <img src="https://images.unsplash.com/photo-1501472312651-726afe119ff1?w=600&auto=format&fit=crop" 
                         onerror="handleArtError(this, 'https://picsum.photos/id/1059/400/500')" 
                         alt="Oceanic Swirl Fluid Painting">
                </div>
                <div class="art-info">
                    <h3 class="art-title">Ethereal Waves</h3>
                    <div class="art-meta">Resin Layer Art • 48" x 48"</div>
                    <div class="art-price">$2,100.00</div>
                    <a href="#" class="btn-action" onclick="alert('Inquiry initialized for: Ethereal Waves')">Inquire to Purchase</a>
                </div>
            </div>

        </div>
    </div>

    <footer>
        <p>&copy; 2026 V.C. Fine Art Studio. Secure Self-Healing Layout Active. Worldwide Shipping Available.</p>
    </footer>

</body>
</html>
...
  # aws-ec2-s3-static-website
Hosting a Static Website with EC2 and S3
