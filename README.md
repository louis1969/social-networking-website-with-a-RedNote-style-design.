# social-networking-website-with-a-RedNote-style-design.
Users can create posts, like content, follow other users, and browse through a feed of posts. 
import React, { useState, useEffect } from 'react';
import { Heart, MessageCircle, Share, Search, Home, User, Bell, Settings, Plus, MoreHorizontal, Send, Bookmark } from 'lucide-react';

const App = () => {
  const [posts, setPosts] = useState([]);
  const [newPost, setNewPost] = useState('');
  const [activeTab, setActiveTab] = useState('home');
  const [likedPosts, setLikedPosts] = useState(new Set());
  const [followedUsers, setFollowedUsers] = useState(new Set());

  // Mock data for posts
  const mockPosts = [
    {
      id: 1,
      user: { name: 'Alex Johnson', username: '@alexj', avatar: 'https://placehold.co/40x40/3b82f6/ffffff?text=AJ' },
      content: 'Just finished an amazing hike in the mountains! The view was absolutely breathtaking. Nature never fails to inspire me.',
      image: 'https://placehold.co/500x300/10b981/ffffff?text=Mountain+View',
      timestamp: '2 hours ago',
      likes: 24,
      comments: 5,
      shares: 2
    },
    {
      id: 2,
      user: { name: 'Sarah Chen', username: '@sarahc', avatar: 'https://placehold.co/40x40/ec4899/ffffff?text=SC' },
      content: 'Working on a new design project today. Sometimes the best ideas come when you step away from the screen and let your mind wander.',
      timestamp: '4 hours ago',
      likes: 18,
      comments: 3,
      shares: 1
    },
    {
      id: 3,
      user: { name: 'Mike Rodriguez', username: '@mikerod', avatar: 'https://placehold.co/40x40/f59e0b/ffffff?text=MR' },
      content: 'Coffee and code - the perfect combination for a productive morning. What\'s your favorite way to start the day?',
      image: 'https://placehold.co/500x300/8b5cf6/ffffff?text=Coffee+%26+Code',
      timestamp: '6 hours ago',
      likes: 31,
      comments: 8,
      shares: 4
    }
  ];

  const mockUsers = [
    { name: 'Emma Wilson', username: '@emmaw', avatar: 'https://placehold.co/40x40/ef4444/ffffff?text=EW' },
    { name: 'David Kim', username: '@davidk', avatar: 'https://placehold.co/40x40/06b6d4/ffffff?text=DK' },
    { name: 'Lisa Park', username: '@lisap', avatar: 'https://placehold.co/40x40/8b5cf6/ffffff?text=LP' }
  ];

  useEffect(() => {
    setPosts(mockPosts);
  }, []);

  const handleLike = (postId) => {
    const newLikedPosts = new Set(likedPosts);
    if (newLikedPosts.has(postId)) {
      newLikedPosts.delete(postId);
    } else {
      newLikedPosts.add(postId);
    }
    setLikedPosts(newLikedPosts);
  };

  const handleFollow = (username) => {
    const newFollowedUsers = new Set(followedUsers);
    if (newFollowedUsers.has(username)) {
      newFollowedUsers.delete(username);
    } else {
      newFollowedUsers.add(username);
    }
    setFollowedUsers(newFollowedUsers);
  };

  const handleCreatePost = () => {
    if (newPost.trim()) {
      const post = {
        id: posts.length + 1,
        user: { name: 'You', username: '@you', avatar: 'https://placehold.co/40x40/6366f1/ffffff?text=Y' },
        content: newPost,
        timestamp: 'now',
        likes: 0,
        comments: 0,
        shares: 0
      };
      setPosts([post, ...posts]);
      setNewPost('');
    }
  };

  const PostCard = ({ post }) => (
    <div className="bg-white rounded-xl shadow-sm border border-gray-100 mb-6 overflow-hidden transition-all duration-200 hover:shadow-md">
      <div className="p-4">
        <div className="flex items-center justify-between mb-3">
          <div className="flex items-center space-x-3">
            <img src={post.user.avatar} alt={post.user.name} className="w-10 h-10 rounded-full" />
            <div>
              <h3 className="font-semibold text-gray-900">{post.user.name}</h3>
              <p className="text-sm text-gray-500">{post.user.username} • {post.timestamp}</p>
            </div>
          </div>
          <button className="p-2 hover:bg-gray-100 rounded-full transition-colors">
            <MoreHorizontal className="w-5 h-5 text-gray-500" />
          </button>
        </div>
        
        <p className="text-gray-800 mb-4 leading-relaxed">{post.content}</p>
        
        {post.image && (
          <img src={post.image} alt="Post content" className="w-full rounded-lg mb-4" />
        )}
        
        <div className="flex items-center justify-between pt-3 border-t border-gray-100">
          <button 
            onClick={() => handleLike(post.id)}
            className={`flex items-center space-x-2 px-3 py-2 rounded-lg transition-colors ${
              likedPosts.has(post.id) ? 'text-red-500 bg-red-50' : 'text-gray-500 hover:bg-gray-100'
            }`}
          >
            <Heart className={`w-5 h-5 ${likedPosts.has(post.id) ? 'fill-current' : ''}`} />
            <span className="text-sm font-medium">{post.likes + (likedPosts.has(post.id) ? 1 : 0)}</span>
          </button>
          
          <button className="flex items-center space-x-2 px-3 py-2 rounded-lg text-gray-500 hover:bg-gray-100 transition-colors">
            <MessageCircle className="w-5 h-5" />
            <span className="text-sm font-medium">{post.comments}</span>
          </button>
          
          <button className="flex items-center space-x-2 px-3 py-2 rounded-lg text-gray-500 hover:bg-gray-100 transition-colors">
            <Share className="w-5 h-5" />
            <span className="text-sm font-medium">{post.shares}</span>
          </button>
          
          <button className="p-2 text-gray-500 hover:bg-gray-100 rounded-lg transition-colors">
            <Bookmark className="w-5 h-5" />
          </button>
        </div>
      </div>
    </div>
  );

  const Sidebar = () => (
    <div className="w-64 pr-6 hidden lg:block">
      <div className="sticky top-6 space-y-6">
        <div className="bg-gradient-to-r from-red-500 to-pink-500 rounded-2xl p-6 text-white">
          <h2 className="text-xl font-bold mb-2">RedNote</h2>
          <p className="text-red-100 text-sm">Connect, share, and discover</p>
        </div>
        
        <div className="space-y-2">
          <button 
            onClick={() => setActiveTab('home')}
            className={`w-full flex items-center space-x-3 px-4 py-3 rounded-xl transition-colors ${
              activeTab === 'home' ? 'bg-red-50 text-red-600' : 'text-gray-700 hover:bg-gray-100'
            }`}
          >
            <Home className="w-5 h-5" />
            <span className="font-medium">Home</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('explore')}
            className={`w-full flex items-center space-x-3 px-4 py-3 rounded-xl transition-colors ${
              activeTab === 'explore' ? 'bg-red-50 text-red-600' : 'text-gray-700 hover:bg-gray-100'
            }`}
          >
            <Search className="w-5 h-5" />
            <span className="font-medium">Explore</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('notifications')}
            className={`w-full flex items-center space-x-3 px-4 py-3 rounded-xl transition-colors ${
              activeTab === 'notifications' ? 'bg-red-50 text-red-600' : 'text-gray-700 hover:bg-gray-100'
            }`}
          >
            <Bell className="w-5 h-5" />
            <span className="font-medium">Notifications</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('profile')}
            className={`w-full flex items-center space-x-3 px-4 py-3 rounded-xl transition-colors ${
              activeTab === 'profile' ? 'bg-red-50 text-red-600' : 'text-gray-700 hover:bg-gray-100'
            }`}
          >
            <User className="w-5 h-5" />
            <span className="font-medium">Profile</span>
          </button>
        </div>
        
        <div className="bg-gray-50 rounded-xl p-4">
          <h3 className="font-semibold text-gray-900 mb-3">Suggested for you</h3>
          <div className="space-y-3">
            {mockUsers.map((user, index) => (
              <div key={index} className="flex items-center justify-between">
                <div className="flex items-center space-x-3">
                  <img src={user.avatar} alt={user.name} className="w-8 h-8 rounded-full" />
                  <div>
                    <p className="text-sm font-medium text-gray-900">{user.name}</p>
                    <p className="text-xs text-gray-500">{user.username}</p>
                  </div>
                </div>
                <button 
                  onClick={() => handleFollow(user.username)}
                  className={`px-3 py-1 text-xs rounded-full font-medium transition-colors ${
                    followedUsers.has(user.username) 
                      ? 'bg-gray-200 text-gray-700' 
                      : 'bg-red-500 text-white hover:bg-red-600'
                  }`}
                >
                  {followedUsers.has(user.username) ? 'Following' : 'Follow'}
                </button>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white border-b border-gray-200 sticky top-0 z-50">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex items-center justify-between h-16">
            <div className="flex items-center space-x-4">
              <div className="flex items-center space-x-2">
                <div className="w-8 h-8 bg-gradient-to-r from-red-500 to-pink-500 rounded-lg flex items-center justify-center">
                  <span className="text-white font-bold text-sm">R</span>
                </div>
                <h1 className="text-xl font-bold text-gray-900">RedNote</h1>
              </div>
            </div>
            
            <div className="flex-1 max-w-lg mx-8">
              <div className="relative">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 w-5 h-5" />
                <input
                  type="text"
                  placeholder="Search..."
                  className="w-full pl-10 pr-4 py-2 bg-gray-100 border-0 rounded-full focus:ring-2 focus:ring-red-500 focus:bg-white transition-all"
                />
              </div>
            </div>
            
            <div className="flex items-center space-x-4">
              <button className="p-2 text-gray-500 hover:text-gray-700 hover:bg-gray-100 rounded-full transition-colors">
                <Settings className="w-5 h-5" />
              </button>
              <img src="https://placehold.co/32x32/6366f1/ffffff?text=Y" alt="Profile" className="w-8 h-8 rounded-full" />
            </div>
          </div>
        </div>
      </header>

      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div className="flex gap-8">
          <Sidebar />
          
          <main className="flex-1">
            {/* Create Post */}
            <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-4 mb-6">
              <div className="flex space-x-3">
                <img src="https://placehold.co/40x40/6366f1/ffffff?text=Y" alt="Your avatar" className="w-10 h-10 rounded-full" />
                <div className="flex-1">
                  <textarea
                    value={newPost}
                    onChange={(e) => setNewPost(e.target.value)}
                    placeholder="What's happening?"
                    className="w-full p-3 border-0 text-gray-900 placeholder-gray-500 resize-none focus:ring-0 text-lg"
                    rows="3"
                  />
                  <div className="flex items-center justify-between pt-3 border-t border-gray-100">
                    <div className="flex space-x-4 text-gray-500">
                      <button className="p-2 hover:bg-gray-100 rounded-full transition-colors">
                        <Plus className="w-5 h-5" />
                      </button>
                    </div>
                    <button
                      onClick={handleCreatePost}
                      disabled={!newPost.trim()}
                      className="px-6 py-2 bg-red-500 text-white rounded-full font-medium hover:bg-red-600 disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
                    >
                      Post
                    </button>
                  </div>
                </div>
              </div>
            </div>

            {/* Posts Feed */}
            <div>
              {posts.map((post) => (
                <PostCard key={post.id} post={post} />
              ))}
            </div>
          </main>
        </div>
      </div>
    </div>
  );
};

export default App;
