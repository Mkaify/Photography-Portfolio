import React, { useState, useEffect, useCallback } from 'react';

// --- DATA ---
const userProfilePic = "https://res.cloudinary.com/dtksqei0m/image/upload/v1760771520/WhatsApp_Image_2025-10-18_at_10.36.14_AM_z1y0lw.jpg";
const galleryImages = [
    // Ensure "Forest path" is prominent
    { src: 'https://images.pexels.com/photos/2254030/pexels-photo-2254030.jpeg', category: 'landscapes', alt: 'Forest path' },
    { src: 'https://images.pexels.com/photos/3225517/pexels-photo-3225517.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'landscapes', alt: 'Mountain Valley' },
    { src: 'https://images.pexels.com/photos/1382731/pexels-photo-1382731.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'portraits', alt: 'Woman in a yellow dress' },
    { src: 'https://images.pexels.com/photos/777059/pexels-photo-777059.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'street', alt: 'City street at night' },
    { src: 'https://images.pexels.com/photos/1024993/pexels-photo-1024993.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'weddings', alt: 'Wedding couple' },
    { src: 'https://images.pexels.com/photos/837358/pexels-photo-837358.jpeg?auto=compress&cs=tinysrgb&w=1260&h-750&dpr=1', category: 'portraits', alt: 'Man in a suit' },
    { src: 'https://images.pexels.com/photos/374710/pexels-photo-374710.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'street', alt: 'Urban architecture' },
    { src: 'https://images.pexels.com/photos/2253870/pexels-photo-2253870.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'weddings', alt: 'Wedding rings' },
    { src: 'https://images.pexels.com/photos/167699/pexels-photo-167699.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'landscapes', alt: 'Misty lake' },
    { src: 'https://images.pexels.com/photos/1043474/pexels-photo-1043474.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'portraits', alt: 'Man with a beard' },
    { src: 'https://images.pexels.com/photos/1105766/pexels-photo-1105766.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'street', alt: 'Neon sign in alley' },
    { src: 'https://images.pexels.com/photos/169198/pexels-photo-169198.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'weddings', alt: 'Wedding reception' },
    { src: 'https://images.pexels.com/photos/2440061/pexels-photo-2440061.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'landscapes', alt: 'Desert Dunes' },
    { src: 'https://images.pexels.com/photos/1310522/pexels-photo-1310522.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'portraits', alt: 'Woman with glasses' },
    { src: 'https://images.pexels.com/photos/2115681/pexels-photo-2115681.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1', category: 'street', alt: 'Person walking in rain' }
];

const albums = ['all', 'landscapes', 'portraits', 'street', 'weddings'];
const initialBio = "Hello! I'm Muhammad Kaif, a final-year Software Engineering student based in Taxila with a deep passion for photography. What started as a hobby has grown into an essential creative outlet alongside my technical studies. I specialize in capturing the fleeting beauty of landscapes and the genuine character in portraits, aiming to create images that tell a story and last a lifetime.";

// --- Gemini API Helper ---
const callGemini = async (payload) => {
    // The API key is handled by the environment, so we leave it empty here.
    const apiKey = ""; 
    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;

    try {
        const response = await fetch(apiUrl, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(payload)
        });

        if (!response.ok) {
            const errorBody = await response.text();
            console.error("API Error Response:", errorBody);
            throw new Error(`API call failed with status: ${response.status}`);
        }

        const result = await response.json();
        const candidate = result.candidates?.[0];
        
        if (candidate && candidate.content?.parts?.[0]?.text) {
            return candidate.content.parts[0].text;
        } else {
            console.error("Unexpected API response structure:", result);
            throw new Error("Failed to extract text from Gemini response.");
        }
    } catch (error) {
        console.error("Error calling Gemini API:", error);
        return "Sorry, something went wrong while generating the content.";
    }
};

// Helper function to convert image URL to Base64
const imageUrlToBase64 = async (url) => {
    try {
        // Use a CORS proxy for pexels.com images
        const proxyUrl = 'https://cors-anywhere.herokuapp.com/';
        const response = await fetch(proxyUrl + url);
        const blob = await response.blob();
        return new Promise((resolve, reject) => {
            const reader = new FileReader();
            reader.onloadend = () => resolve(reader.result.split(',')[1]);
            reader.onerror = reject;
            reader.readAsDataURL(blob);
        });
    } catch (error) {
        console.error("Error converting image to Base64:", error);
        return null;
    }
};


// --- Reusable Components ---

const Sidebar = ({ currentFilter, onFilterChange, onAboutClick, onContactClick, isOpen, setIsOpen }) => {
    const handleFilterClick = (filter) => {
        onFilterChange(filter);
        if (window.innerWidth < 1024) {
             setIsOpen(false);
        }
    }
    
    return (
        <aside className={`bg-black/80 backdrop-blur-sm w-64 p-6 space-y-8 flex-shrink-0 fixed lg:relative h-full transition-transform duration-300 z-40 ${isOpen ? 'translate-x-0' : '-translate-x-full'} lg:translate-x-0`}>
            <div>
                <h1 className="text-2xl font-bold text-white tracking-wider">MK Photography</h1>
                <p className="text-sm text-gray-400">Muhammad Kaif</p>
            </div>
            
            <nav>
                <h2 className="text-xs font-semibold text-gray-500 uppercase tracking-widest mb-4">Albums</h2>
                <div className="space-y-2">
                    {albums.map(album => (
                        <a 
                            href="#" 
                            key={album} 
                            onClick={(e) => { e.preventDefault(); handleFilterClick(album); }}
                            className={`block font-medium capitalize transition-colors ${currentFilter === album ? 'text-white' : 'text-gray-300 hover:text-white'}`}
                        >
                            {album === 'all' ? 'All Photos' : album}
                        </a>
                    ))}
                </div>
            </nav>

            <div className="border-t border-gray-800 pt-6">
                <h2 className="text-xs font-semibold text-gray-500 uppercase tracking-widest mb-4">Menu</h2>
                 <div className="space-y-2">
                    <a href="#" onClick={(e) => { e.preventDefault(); onAboutClick(); }} className="block font-medium text-gray-300 hover:text-white">About</a>
                    <a href="#" onClick={(e) => { e.preventDefault(); onContactClick(); }} className="block font-medium text-gray-300 hover:text-white">Contact</a>
                </div>
            </div>
        </aside>
    );
};

const Gallery = ({ images, onImageClick }) => (
    <div id="gallery-grid" className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-1 p-1">
        {images.map((image, index) => (
            <div 
                key={image.src + index} 
                className="aspect-square bg-gray-900 cursor-pointer transition-transform duration-200 ease-in-out hover:scale-95 hover:opacity-80" 
                onClick={() => onImageClick(index)}
            >
                <img src={image.src} alt={image.alt} className="w-full h-full object-cover" loading="lazy" />
            </div>
        ))}
    </div>
);

const Lightbox = ({ image, onClose, onPrev, onNext, hasPrev, hasNext }) => {
    const [isClosing, setIsClosing] = useState(false);
    const [generatedCaption, setGeneratedCaption] = useState('');
    const [isGenerating, setIsGenerating] = useState(false);

    const handleClose = useCallback(() => {
        setIsClosing(true);
        setTimeout(() => {
            onClose();
            setGeneratedCaption(''); // Reset on close
        }, 300);
    }, [onClose]);
    
    const handleGenerateDescription = async () => {
        if (!image) return;
        setIsGenerating(true);
        setGeneratedCaption('');
        
        const base64ImageData = await imageUrlToBase64(image.src);

        if (base64ImageData) {
            const payload = {
                contents: [{
                    parts: [
                        { text: "Describe this image for a photography portfolio. Be descriptive and evocative. This will be used as a caption." },
                        { inlineData: { mimeType: "image/jpeg", data: base64ImageData } }
                    ]
                }]
            };
            const description = await callGemini(payload);
            setGeneratedCaption(description);
        } else {
            setGeneratedCaption("Could not analyze the image. This might be due to CORS policy restrictions on the image server.");
        }
        setIsGenerating(false);
    };

    // Reset generated caption when image changes
    useEffect(() => {
        setGeneratedCaption('');
        setIsGenerating(false);
    }, [image]);

    // Keyboard navigation
    useEffect(() => {
        const handleKeyDown = (e) => {
            if (e.key === 'Escape') handleClose();
            if (e.key === 'ArrowLeft' && hasPrev) onPrev();
            if (e.key === 'ArrowRight' && hasNext) onNext();
        };
        window.addEventListener('keydown', handleKeyDown);
        return () => window.removeEventListener('keydown', handleKeyDown);
    }, [hasPrev, hasNext, onPrev, onNext, handleClose]);

    if (!image) return null;

    return (
        <div 
            className={`fixed inset-0 bg-black/90 z-50 flex items-center justify-center p-4 transition-opacity duration-300 ${isClosing ? 'opacity-0' : 'opacity-100'}`}
            onClick={handleClose}
        >
            <div className="relative w-full h-full flex items-center justify-center" onClick={(e) => e.stopPropagation()}>
                <img 
                    src={image.src}
                    alt={generatedCaption || image.alt}
                    className="max-w-full max-h-full object-contain rounded-lg shadow-2xl transition-transform transform duration-300 scale-100"
                />
                <button onClick={handleClose} className="absolute top-4 right-4 text-white text-3xl font-light hover:text-gray-300 z-10">&times;</button>
                
                {hasPrev && (
                    <button onClick={onPrev} className="absolute left-4 top-1/2 -translate-y-1/2 text-white bg-black/20 p-3 rounded-full hover:bg-black/50">
                        <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M15 19l-7-7 7-7"></path></svg>
                    </button>
                )}

                {hasNext && (
                    <button onClick={onNext} className="absolute right-4 top-1/2 -translate-y-1/2 text-white bg-black/20 p-3 rounded-full hover:bg-black/50">
                        <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 5l7 7-7 7"></path></svg>
                    </button>
                )}

                <div className="absolute bottom-4 left-4 right-4 text-white text-center text-sm p-3 bg-black/50 rounded-md space-y-2">
                    <p>{isGenerating ? 'Generating description...' : (generatedCaption || image.alt)}</p>
                    <button 
                        onClick={handleGenerateDescription} 
                        disabled={isGenerating}
                        className="bg-blue-600 hover:bg-blue-700 disabled:bg-gray-500 text-white font-semibold py-1 px-3 text-xs rounded-full transition duration-300"
                    >
                        ✨ Describe Image with AI
                    </button>
                </div>
            </div>
        </div>
    );
};

const AboutModal = ({ isOpen, onClose }) => {
    const [bio, setBio] = useState(initialBio);
    const [isGenerating, setIsGenerating] = useState(false);

    const handleGenerateBio = async () => {
        setIsGenerating(true);
        const prompt = "You are a professional copywriter. Write a compelling and personal 'About Me' bio for a photography portfolio. The photographer's name is Muhammad Kaif. He is a final-year Software Engineering student from Taxila with a deep passion for photography. Mention that he specializes in landscapes and portraits and aims to tell stories with his images. Keep it concise, warm, and engaging (around 3-4 sentences).";
        const payload = { contents: [{ parts: [{ text: prompt }] }] };
        const newBio = await callGemini(payload);
        setBio(newBio);
        setIsGenerating(false);
    };
    
    useEffect(() => {
        if(isOpen) {
            setBio(initialBio); // Reset bio when modal opens
        }
    }, [isOpen]);

    if (!isOpen) return null;
    return (
        <div className="fixed inset-0 bg-black/90 z-50 flex items-center justify-center p-6" onClick={onClose}>
            <div className="bg-[#1a1a1a] p-8 rounded-lg max-w-2xl w-full relative text-center" onClick={e => e.stopPropagation()}>
                <button onClick={onClose} className="absolute top-4 right-4 text-gray-500 hover:text-white">&times;</button>
                <img src={userProfilePic} alt="Muhammad Kaif" className="w-24 h-24 rounded-full mx-auto mb-4 ring-2 ring-gray-700 object-cover"/>
                <h2 className="text-3xl font-bold text-white mb-4">About Me</h2>
                <p className="text-gray-400 leading-relaxed min-h-[100px]">
                    {isGenerating ? 'Generating new bio...' : bio}
                </p>
                <button 
                    onClick={handleGenerateBio} 
                    disabled={isGenerating}
                    className="mt-6 bg-blue-600 hover:bg-blue-700 disabled:bg-gray-500 text-white font-bold py-2 px-4 rounded-full transition duration-300"
                >
                    ✨ Generate Bio with AI
                </button>
            </div>
        </div>
    );
};

const ContactModal = ({ isOpen, onClose }) => {
    const [message, setMessage] = useState('');

    const handleSubmit = (e) => {
        e.preventDefault();
        setMessage('Message sent successfully!');
        e.target.reset();
        setTimeout(() => {
            setMessage('');
            onClose();
        }, 3000);
    };
    
    if (!isOpen) return null;

    return (
        <div className="fixed inset-0 bg-black/90 z-50 flex items-center justify-center p-6" onClick={onClose}>
             <div className="bg-[#1a1a1a] p-8 rounded-lg max-w-lg w-full relative" onClick={e => e.stopPropagation()}>
                 <button onClick={onClose} className="absolute top-4 right-4 text-gray-500 hover:text-white">&times;</button>
                 <h2 className="text-3xl font-bold text-white mb-4 text-center">Get In Touch</h2>
                 <form onSubmit={handleSubmit} className="space-y-4">
                     <input type="text" placeholder="Your Name" required className="w-full bg-gray-800 text-white p-3 rounded-md border border-gray-700 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"/>
                     <input type="email" placeholder="Your Email" required className="w-full bg-gray-800 text-white p-3 rounded-md border border-gray-700 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"/>
                     <textarea rows="4" placeholder="Your Message" required className="w-full bg-gray-800 text-white p-3 rounded-md border border-gray-700 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"></textarea>
                     <button type="submit" className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-md transition duration-300">Send Message</button>
                 </form>
                 {message && <div className="mt-4 text-center text-green-400">{message}</div>}
             </div>
        </div>
    );
};


// --- Main App Component ---
export default function HomePage() {
    // State for filtering
    const [currentFilter, setCurrentFilter] = useState('all');
    const [filteredImages, setFilteredImages] = useState(galleryImages);

    // State for modals and lightbox
    const [lightboxIndex, setLightboxIndex] = useState(null);
    const [isAboutOpen, setIsAboutOpen] = useState(false);
    const [isContactOpen, setIsContactOpen] = useState(false);
    
    // State for mobile sidebar
    const [isSidebarOpen, setIsSidebarOpen] = useState(false);

    // Effect to update gallery when filter changes
    useEffect(() => {
        if (currentFilter === 'all') {
            setFilteredImages(galleryImages);
        } else {
            setFilteredImages(galleryImages.filter(img => img.category === currentFilter));
        }
    }, [currentFilter]);

    // Effect to lock body scroll when a modal is open
    useEffect(() => {
        const isModalOpen = lightboxIndex !== null || isAboutOpen || isContactOpen;
        document.body.style.overflow = isModalOpen ? 'hidden' : 'auto';
    }, [lightboxIndex, isAboutOpen, isContactOpen]);

    const handleOpenLightbox = (index) => setLightboxIndex(index);
    const handleCloseLightbox = () => setLightboxIndex(null);

    const handlePrevImage = useCallback(() => {
        setLightboxIndex(prevIndex => (prevIndex > 0 ? prevIndex - 1 : prevIndex));
    }, []);

    const handleNextImage = useCallback(() => {
        setLightboxIndex(prevIndex => (prevIndex < filteredImages.length - 1 ? prevIndex + 1 : prevIndex));
    }, [filteredImages.length]);

    return (
        <>
            {/* The Head component and global styles are handled in _app.js and globals.css */}
            
            <div className="flex h-screen bg-[#0a0a0a] text-gray-200">
                <Sidebar 
                    currentFilter={currentFilter}
                    onFilterChange={setCurrentFilter}
                    onAboutClick={() => setIsAboutOpen(true)}
                    onContactClick={() => setIsContactOpen(true)}
                    isOpen={isSidebarOpen}
                    setIsOpen={setIsSidebarOpen}
                />

                <main className="flex-1 bg-[#0a0a0a] overflow-y-auto">
                     <header className="sticky top-0 bg-[#0a0a0a]/80 backdrop-blur-sm p-4 z-30 flex items-center justify-between lg:hidden">
                        <h1 className="text-lg font-bold text-white">MK Photography</h1>
                        <button onClick={() => setIsSidebarOpen(!isSidebarOpen)} className="p-2">
                            <svg className="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
                        </button>
                    </header>
                    <Gallery images={filteredImages} onImageClick={handleOpenLightbox} />
                </main>
            </div>

            {lightboxIndex !== null && (
                 <Lightbox 
                    image={filteredImages[lightboxIndex]}
                    onClose={handleCloseLightbox}
                    onPrev={handlePrevImage}
                    onNext={handleNextImage}
                    hasPrev={lightboxIndex > 0}
                    hasNext={lightboxIndex < filteredImages.length - 1}
                 />
            )}
            
            <AboutModal isOpen={isAboutOpen} onClose={() => setIsAboutOpen(false)} />
            <ContactModal isOpen={isContactOpen} onClose={() => setIsContactOpen(false)} />
        </>
    );
}

