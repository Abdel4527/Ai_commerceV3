import React, { useEffect, useRef, useState } from 'react';
import { FiChevronLeft, FiChevronRight } from 'react-icons/fi';
import { Link } from 'react-router-dom';

const NewArrivals = () => {
    const scrollRef = useRef(null);
    const [canScrollLeft, setCanScrollLeft] = useState(false);
    const [canScrollRight, setCanScrollRight] = useState(true);

    const newArrivals = [
        {
            id: 1,
            name: 'Product 1',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=1",
                    altText: "Product 1",
                }
            ]
        },
        {
            id: 2,
            name: 'Product 2',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=2",
                    altText: "Product 2",
                }
            ]
        },
        {
            id: 3,
            name: 'Product 3',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=3",
                    altText: "Product 3",
                }
            ]
        },
        {
            id: 4,
            name: 'Product 4',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=4",
                    altText: "Product 4",
                }
            ]
        },
        {
            id: 5,
            name: 'Product 5',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=5",
                    altText: "Product 5",
                }
            ]
        },
        {
            id: 6,
            name: 'Product 6',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=6",
                    altText: "Product 6",
                }
            ]
        },
        {
            id: 7,
            name: 'Product 7',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=7",
                    altText: "Product 7",
                }
            ]
        },
        {
            id: 8,
            name: 'Product 8',
            price: 100,
            images: [
                {
                    url: "https://picsum.photos/500/500?random=8",
                    altText: "Product 8",
                }
            ]
        }
    ];
    const handleMouseDown = (e) => {
        setIsDragging(true);
        setStartX(e.pageX - scrollRef.current.offsetLeft);
        setCanScrollLeft(scrollRef.current.scrollLeft);
    };

    const scroll = (direction) => {
        const scrollAmount = direction === "left" ? -300 : 300;
        scrollRef.current.scrollBy({left: scrollAmount, behavior: "smooth"});

    };

    const handleMouseMove = (e) => {
        if(!isDragging) return;
        const x = e.pageX - scrollRef.current.offsetLeft;
        const walk = x - startX;
        scrollRef.current.scrollLeft = canScrollLeft - walk;
    };

    const handleMouseUp0rLeave = () => {
        setIsDragging(false);   
    };

    const updateScrollButtons = () => {
        const container = scrollRef.current;
        if(container){
            const leftScroll = container.scrollLeft;
            const rightScrollable = container.scrollWidth > leftScroll + container.clientWidth;
            setCanScrollLeft(leftScroll > 0);
            setCanScrollRight(rightScrollable);
        }

        console.log({
            scrollLeft: container.scrollLeft,
            conatinerScrollWidth: container.scrollWidth,
            clientWidth: container.clientWidth,
            offsetLeft: scrollRef.current.offsetLeft
        })
    };

    useEffect(() => {
        const container = scrollRef.current;
        if(container) {
            container.addEventListener("scroll", updateScrollButtons);
            updateScrollButtons();
            
            return () => {
                container.removeEventListener("scroll", updateScrollButtons);
            };
        }
    }, []);

    return (
        <section>
            <div className="container mx-auto text-center mb-10 relative">
                <h2 className="text-3xl font-bold text-gray-900 mb-4">
                    New Arrivals
                </h2>
                <p className="text-gray-600 mb-8 text-lg">
                    Check out our latest products.
                </p>
                <div className="absolute right-0 bottom-[-30px] flex space-x-2">
                    <button 
                        onClick={() => scroll("left")} 
                        disabled={!canScrollLeft} 
                        className={`p-2 rounded border ${canScrollLeft ? "bg-white text-black" : "bg-gray-200 text-gray-400 cursor-not-allowed"}`}
                    >
                        <FiChevronLeft className="text-2xl"/>
                    </button>
                    <button 
                        onClick={() => scroll("right")} 
                        disabled={!canScrollRight} 
                        className={`p-2 rounded border ${canScrollRight ? "bg-white text-black" : "bg-gray-200 text-gray-400 cursor-not-allowed"}`}
                    >
                        <FiChevronRight className="text-2xl"/>
                    </button>
                </div>
            </div>
            {/*scrollable content*/}
            <div ref={scrollRef} className="container mx-auto overflow-x-scroll flex space-x-6 relative" 
                onMouseDown={handleMouseDown}
                onMouseMove={handleMouseMove}
                onMouseUp={handleMouseUp0rLeave}
                onMouseLeave={handleMouseUp0rLeave}
             >
                {newArrivals.map((product) => (
                    <div key={product.id} className="min-w-[100%] sm:min-w-[50%] lg:min-w-[30%] relative">
                        <img 
                            src={product.images[0]?.url} 
                            alt={product.images[0]?.altText || product.name} 
                            className="w-full h-[500px] object-cover rounded-lg"
                            draggable={false} 
                        />
                        <div className="absolute bottom-0 left-0 right-0  bg-opacity-50 backdrop-blur-md text-white p-4 rounded-b-lg">
                            <Link to={`/product/${product.id}`} className="block">
                                <h4 className="font-medium">
                                    {product.name}
                                </h4>
                                <p className="mt-1">${product.price}</p>
                            </Link>
                        </div>
                    </div>
                ))}
            </div>
        </section>
    );
};

export default NewArrivals;